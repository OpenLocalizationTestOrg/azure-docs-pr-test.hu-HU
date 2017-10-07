---
title: "Oktatóanyag: Azure Active Directory-integráció a Google Apps, az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Google alkalmazások között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 2093b5ab605ec0d7bbefe7a78e1eede34d756f53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-google-apps"></a><span data-ttu-id="820fd-103">Oktatóanyag: Azure Active Directory-integráció a Google Apps</span><span class="sxs-lookup"><span data-stu-id="820fd-103">Tutorial: Azure Active Directory integration with Google Apps</span></span>

<span data-ttu-id="820fd-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Google Apps, az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="820fd-104">In this tutorial, you learn how toointegrate Google Apps with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="820fd-105">Google Apps alkalmazások integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="820fd-105">Integrating Google Apps with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="820fd-106">Megadhatja a hozzáférés tooGoogle alkalmazások rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="820fd-106">You can control in Azure AD who has access tooGoogle Apps</span></span>
- <span data-ttu-id="820fd-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooGoogle (egyszeri bejelentkezés) alkalmazásokat a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="820fd-107">You can enable your users tooautomatically get signed-on tooGoogle Apps (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="820fd-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="820fd-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="820fd-109">Ha szeretne tooknow az Azure AD SaaS integrálásáról további információt, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="820fd-109">If you want tooknow more information about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="820fd-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="820fd-110">Prerequisites</span></span>

<span data-ttu-id="820fd-111">tooconfigure az Azure AD integrálása a Google Apps, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="820fd-111">tooconfigure Azure AD integration with Google Apps, you need hello following items:</span></span>

- <span data-ttu-id="820fd-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="820fd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="820fd-113">A Google Apps egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="820fd-113">A Google Apps single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="820fd-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="820fd-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="820fd-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="820fd-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="820fd-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="820fd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="820fd-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="820fd-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="video-tutorial"></a><span data-ttu-id="820fd-118">Oktatóvideó</span><span class="sxs-lookup"><span data-stu-id="820fd-118">Video tutorial</span></span>
<span data-ttu-id="820fd-119">Hogyan tooEnable egyszeri bejelentkezés tooGoogle alkalmazások két percen belül:</span><span class="sxs-lookup"><span data-stu-id="820fd-119">How tooEnable Single Sign-On tooGoogle Apps in 2 Minutes:</span></span>

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enable-single-sign-on-to-Google-Apps-in-2-minutes-with-Azure-AD/player]

## <a name="frequently-asked-questions"></a><span data-ttu-id="820fd-120">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="820fd-120">Frequently Asked Questions</span></span>
1. <span data-ttu-id="820fd-121">**K: Chromebooks és egyéb Chrome eszközök kompatibilisek legyenek az Azure AD az egyszeri bejelentkezés?**</span><span class="sxs-lookup"><span data-stu-id="820fd-121">**Q: Are Chromebooks and other Chrome devices compatible with Azure AD single sign-on?**</span></span>
   
    <span data-ttu-id="820fd-122">A: felhasználók Igen, képes toosign be a Azure AD hitelesítő adataik használatával Chromebook eszközeik.</span><span class="sxs-lookup"><span data-stu-id="820fd-122">A: Yes, users are able toosign into their Chromebook devices using their Azure AD credentials.</span></span> <span data-ttu-id="820fd-123">Ez [Google alkalmazások támogatják a cikk](https://support.google.com/chrome/a/answer/6060880) információt arról, hogy miért felhasználók lehet kérni a hitelesítő adatok kétszer.</span><span class="sxs-lookup"><span data-stu-id="820fd-123">See this [Google Apps support article](https://support.google.com/chrome/a/answer/6060880) for information on why users may get prompted for credentials twice.</span></span>

2. <span data-ttu-id="820fd-124">**Kérdés: Ha az egyszeri bejelentkezés engedélyezése a felhasználók fog tudni toouse kell az Azure AD hitelesítő adatait toosign történő bármely Google termék, például a Google osztályteremben, GMail, Google meghajtó, YouTube stb?**</span><span class="sxs-lookup"><span data-stu-id="820fd-124">**Q: If I enable single sign-on, will users be able toouse their Azure AD credentials toosign into any Google product, such as Google Classroom, GMail, Google Drive, YouTube, and so on?**</span></span>
   
    <span data-ttu-id="820fd-125">V: Igen, attól függően [mely Google apps](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) tooenable válasszon, vagy tiltsa le a szervezet számára.</span><span class="sxs-lookup"><span data-stu-id="820fd-125">A: Yes, depending on [which Google apps](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) you choose tooenable or disable for your organization.</span></span>

3. <span data-ttu-id="820fd-126">**K: engedélyezhető az egyszeri bejelentkezéshez csak egy része a Google Apps-alkalmazások felhasználók számára?**</span><span class="sxs-lookup"><span data-stu-id="820fd-126">**Q: Can I enable single sign-on for only a subset of my Google Apps users?**</span></span>
   
    <span data-ttu-id="820fd-127">A: nem bekapcsolásával egyszeri bejelentkezés azonnal szükséges összes a Google Apps felhasználók tooauthenticate az Azure AD hitelesítő adataikkal.</span><span class="sxs-lookup"><span data-stu-id="820fd-127">A: No, turning on single sign-on immediately requires all your Google Apps users tooauthenticate with their Azure AD credentials.</span></span> <span data-ttu-id="820fd-128">Google Apps nem támogatja több identitás-szolgáltatóktól rendelkező, mert a Google Apps környezetnek hello identitásszolgáltató lehet az Azure AD vagy Google –, de egyszerre csak hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="820fd-128">Because Google Apps doesn't support having multiple identity providers, hello identity provider for your Google Apps environment can either be Azure AD or Google -- but not both at hello same time.</span></span>

4. <span data-ttu-id="820fd-129">**K:, ha van bejelentkezett felhasználó Windows keresztül,, azok automatikusan tooGoogle alkalmazások használata nélkül végezzen hitelesítést a rendszer első jelszót?**</span><span class="sxs-lookup"><span data-stu-id="820fd-129">**Q: If a user is signed in through Windows, are they automatically authenticate tooGoogle Apps without getting prompted for a password?**</span></span>
   
    <span data-ttu-id="820fd-130">A: Ez a forgatókönyv engedélyezésének két lehetőség áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="820fd-130">A: There are two options for enabling this scenario.</span></span> <span data-ttu-id="820fd-131">Először sikerült jelentkeznek be Windows 10-eszközöket az [Azure Active Directory csatlakozási](active-directory-azureadjoin-overview.md).</span><span class="sxs-lookup"><span data-stu-id="820fd-131">First, users could sign into Windows 10 devices via [Azure Active Directory Join](active-directory-azureadjoin-overview.md).</span></span> <span data-ttu-id="820fd-132">Azt is megteheti, sikerült jelentkeznek be Windows-eszközök, amelyek tooan tartományhoz a helyszíni Active Directoryban, amely az egyszeri bejelentkezés tooAzure AD keresztül engedélyezve van egy [Active Directory összevonási szolgáltatások (AD FS)](active-directory-aadconnect-user-signin.md) központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="820fd-132">Alternatively, users could sign into Windows devices that are domain-joined tooan on-premises Active Directory that has been enabled for single sign-on tooAzure AD via an [Active Directory Federation Services (AD FS)](active-directory-aadconnect-user-signin.md) deployment.</span></span> <span data-ttu-id="820fd-133">Mindkét lehetőség kell tooperform hello lépéseit az oktatóanyag tooenable egyszeri bejelentkezés az Azure AD között a következő hello és a Google Apps.</span><span class="sxs-lookup"><span data-stu-id="820fd-133">Both options require you tooperform hello steps in hello following tutorial tooenable single sign-on between Azure AD and Google Apps.</span></span>

## <a name="scenario-description"></a><span data-ttu-id="820fd-134">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="820fd-134">Scenario description</span></span>
<span data-ttu-id="820fd-135">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="820fd-135">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="820fd-136">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="820fd-136">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="820fd-137">Google Apps hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="820fd-137">Adding Google Apps from hello gallery</span></span>
2. <span data-ttu-id="820fd-138">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="820fd-138">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-google-apps-from-hello-gallery"></a><span data-ttu-id="820fd-139">Google Apps hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="820fd-139">Adding Google Apps from hello gallery</span></span>
<span data-ttu-id="820fd-140">tooconfigure hello integráció a Google Apps, az Azure AD-be, meg kell tooadd Google Apps hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="820fd-140">tooconfigure hello integration of Google Apps into Azure AD, you need tooadd Google Apps from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="820fd-141">**tooadd Google Apps hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="820fd-141">**tooadd Google Apps from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="820fd-142">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="820fd-142">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="820fd-144">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="820fd-144">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="820fd-145">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="820fd-145">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="820fd-147">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="820fd-147">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="820fd-149">Hello keresési mezőbe, írja be a **Google Apps**.</span><span class="sxs-lookup"><span data-stu-id="820fd-149">In hello search box, type **Google Apps**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_search.png)

5. <span data-ttu-id="820fd-151">A hello eredmények panelen válassza ki a **Google Apps**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="820fd-151">In hello results panel, select **Google Apps**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="820fd-153">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="820fd-153">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="820fd-154">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Google Apps "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="820fd-154">In this section, you configure and test Azure AD single sign-on with Google Apps based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="820fd-155">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Google Apps tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="820fd-155">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Google Apps is tooa user in Azure AD.</span></span> <span data-ttu-id="820fd-156">Ez azt jelenti hello kapcsolódó felhasználó a Google Apps és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="820fd-156">In other words, a link relationship between an Azure AD user and hello related user in Google Apps needs toobe established.</span></span>

<span data-ttu-id="820fd-157">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Google Apps.</span><span class="sxs-lookup"><span data-stu-id="820fd-157">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Google Apps.</span></span>

<span data-ttu-id="820fd-158">tooconfigure és az Azure AD az egyszeri bejelentkezés Google Apps-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="820fd-158">tooconfigure and test Azure AD single sign-on with Google Apps, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="820fd-159">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="820fd-159">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="820fd-160">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="820fd-160">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="820fd-161">**[Google Apps tesztfelhasználó létrehozása](#creating-a-google-apps-test-user)**  -toohave egy megfelelője a Britta Simon a Google Apps, amely a felhasználó csatolt toohello az Azure AD-ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="820fd-161">**[Creating a Google Apps test user](#creating-a-google-apps-test-user)** - toohave a counterpart of Britta Simon in Google Apps that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="820fd-162">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="820fd-162">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="820fd-163">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="820fd-163">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="820fd-164">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="820fd-164">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="820fd-165">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a Google Apps-alkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="820fd-165">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Google Apps application.</span></span>

<span data-ttu-id="820fd-166">**az Azure AD tooconfigure egyszeri bejelentkezést a Google Apps, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="820fd-166">**tooconfigure Azure AD single sign-on with Google Apps, perform hello following steps:**</span></span>

1. <span data-ttu-id="820fd-167">Az Azure portál, a hello hello **Google Apps** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="820fd-167">In hello Azure portal, on hello **Google Apps** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="820fd-169">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="820fd-169">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_samlbase.png)

3. <span data-ttu-id="820fd-171">A hello **Google Apps tartományi és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="820fd-171">On hello **Google Apps Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_url.png)

    <span data-ttu-id="820fd-173">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://mail.google.com/a/<yourdomain>`</span><span class="sxs-lookup"><span data-stu-id="820fd-173">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://mail.google.com/a/<yourdomain>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="820fd-174">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="820fd-174">This value is not real.</span></span> <span data-ttu-id="820fd-175">Frissítse a hello érték hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="820fd-175">Update hello value with hello actual Sign-on URL.</span></span> <span data-ttu-id="820fd-176">Lépjen kapcsolatba a hello [Google támogatási csoport](https://www.google.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="820fd-176">contact hello [Google support team](https://www.google.com/contact/).</span></span>
 
4. <span data-ttu-id="820fd-177">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány** , és mentse a hello tanúsítvány a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="820fd-177">On hello **SAML Signing Certificate** section, click **Certificate** and then save hello certificate on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_certificate.png) 

5. <span data-ttu-id="820fd-179">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="820fd-179">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-google-apps-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="820fd-181">A hello **Google Apps konfigurációs** területén kattintson **konfigurálása Google Apps** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="820fd-181">On hello **Google Apps Configuration** section, click **Configure Google Apps** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="820fd-182">Másolás hello **Sign-Out URL-címet, a SAML-alapú egyszeri bejelentkezési URL-címe és a módosítás jelszó URL-cím** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="820fd-182">Copy hello **Sign-Out URL, SAML Single Sign-On Service URL and Change password URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_configure.png) 

7. <span data-ttu-id="820fd-184">Új lap megnyitása a böngészőben, és jelentkezzen be a hello [Google Apps felügyeleti konzol](http://admin.google.com/) a rendszergazdai fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="820fd-184">Open a new tab in your browser, and sign into hello [Google Apps Admin Console](http://admin.google.com/) using your administrator account.</span></span>

8. <span data-ttu-id="820fd-185">Kattintson a **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="820fd-185">Click **Security**.</span></span> <span data-ttu-id="820fd-186">Ha hello hivatkozás nem látható, akkor előfordulhat, hogy rejtve a hello **további vezérlők** menü üdvözlő képernyőt hello alján.</span><span class="sxs-lookup"><span data-stu-id="820fd-186">If you don't see hello link, it may be hidden under hello **More Controls** menu at hello bottom of hello screen.</span></span>
   
    ![Kattintson a Security (Biztonság) elemre.][10]

9. <span data-ttu-id="820fd-188">A hello **biztonsági** kattintson **beállítása az egyszeri bejelentkezés (SSO).**</span><span class="sxs-lookup"><span data-stu-id="820fd-188">On hello **Security** page, click **Set up single sign-on (SSO).**</span></span>
   
    ![Kattintson az egyszeri Bejelentkezést.][11]

10. <span data-ttu-id="820fd-190">Hajtsa végre a következő konfigurációs módosításainak hello:</span><span class="sxs-lookup"><span data-stu-id="820fd-190">Perform hello following configuration changes:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][12]
   
    <span data-ttu-id="820fd-192">a.</span><span class="sxs-lookup"><span data-stu-id="820fd-192">a.</span></span> <span data-ttu-id="820fd-193">Válassza ki **külső identitásszolgáltatótól telepítő SSO**.</span><span class="sxs-lookup"><span data-stu-id="820fd-193">Select **Setup SSO with third-party identity provider**.</span></span>

    <span data-ttu-id="820fd-194">b.</span><span class="sxs-lookup"><span data-stu-id="820fd-194">b.</span></span> <span data-ttu-id="820fd-195">A a **bejelentkezési URL-címe** Google Apps mezőbe illessze be a hello értékének **egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="820fd-195">In the **Sign-in page URL** field in Google Apps, paste hello value of **Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="820fd-196">c.</span><span class="sxs-lookup"><span data-stu-id="820fd-196">c.</span></span> <span data-ttu-id="820fd-197">A hello **kijelentkezési URL-címe** Google Apps mezőbe illessze be a hello értékének **Sign-Out URL-cím**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="820fd-197">In hello **Sign-out page URL** field in Google Apps, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="820fd-198">d.</span><span class="sxs-lookup"><span data-stu-id="820fd-198">d.</span></span> <span data-ttu-id="820fd-199">A hello **Módosítsa jelszavát URL-címet** Google Apps mezőbe illessze be a hello értékének **Módosítsa jelszavát URL-címet**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="820fd-199">In hello **Change password URL** field in Google Apps, paste hello value of **Change password URL**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="820fd-200">e.</span><span class="sxs-lookup"><span data-stu-id="820fd-200">e.</span></span> <span data-ttu-id="820fd-201">A Google Apps, az hello **ellenőrző tanúsítvány**, az Azure-portálról letöltött feltöltés hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="820fd-201">In Google Apps, for hello **Verification certificate**, upload hello certificate that you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="820fd-202">f.</span><span class="sxs-lookup"><span data-stu-id="820fd-202">f.</span></span> <span data-ttu-id="820fd-203">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="820fd-203">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="820fd-204">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="820fd-204">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="820fd-205">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="820fd-205">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="820fd-206">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="820fd-206">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="820fd-207">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="820fd-207">Creating an Azure AD test user</span></span>
<span data-ttu-id="820fd-208">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="820fd-208">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="820fd-210">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="820fd-210">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="820fd-211">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="820fd-211">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-google-apps-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="820fd-213">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="820fd-213">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-google-apps-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="820fd-215">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="820fd-215">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-google-apps-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="820fd-217">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="820fd-217">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-google-apps-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="820fd-219">a.</span><span class="sxs-lookup"><span data-stu-id="820fd-219">a.</span></span> <span data-ttu-id="820fd-220">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="820fd-220">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="820fd-221">b.</span><span class="sxs-lookup"><span data-stu-id="820fd-221">b.</span></span> <span data-ttu-id="820fd-222">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="820fd-222">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="820fd-223">c.</span><span class="sxs-lookup"><span data-stu-id="820fd-223">c.</span></span> <span data-ttu-id="820fd-224">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="820fd-224">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="820fd-225">d.</span><span class="sxs-lookup"><span data-stu-id="820fd-225">d.</span></span> <span data-ttu-id="820fd-226">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="820fd-226">Click **Create**.</span></span>
 
### <a name="creating-a-google-apps-test-user"></a><span data-ttu-id="820fd-227">Google Apps tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="820fd-227">Creating a Google Apps test user</span></span>

<span data-ttu-id="820fd-228">hello ebben a szakaszban célja toocreate a Google Apps szoftver Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="820fd-228">hello objective of this section is toocreate a user called Britta Simon in Google Apps Software.</span></span> <span data-ttu-id="820fd-229">Google Apps támogatja az Automatikus kiépítés, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="820fd-229">Google Apps supports auto provisioning, which is by default enabled.</span></span> <span data-ttu-id="820fd-230">Nincs olyan művelet, ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="820fd-230">There is no action for you in this section.</span></span> <span data-ttu-id="820fd-231">Ha a felhasználó nem létezik a Google Apps szoftver, egy új tooaccess Google Apps szoftver tett kísérlet során jön létre.</span><span class="sxs-lookup"><span data-stu-id="820fd-231">If a user doesn't already exist in Google Apps Software, a new one is created when you attempt tooaccess Google Apps Software.</span></span>

>[!NOTE] 
><span data-ttu-id="820fd-232">Ha egy felhasználó toocreate manuálisan kell, lépjen kapcsolatba a hello [Google támogatási csoport](https://www.google.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="820fd-232">If you need toocreate a user manually, contact hello [Google support team](https://www.google.com/contact/).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="820fd-233">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="820fd-233">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="820fd-234">Ebben a szakaszban Azure egyszeri bejelentkezés Britta Simon toouse tooGoogle alkalmazásokat nyújtó engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="820fd-234">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooGoogle Apps.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="820fd-236">**tooassign Britta Simon tooGoogle alkalmazások, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="820fd-236">**tooassign Britta Simon tooGoogle Apps, perform hello following steps:**</span></span>

1. <span data-ttu-id="820fd-237">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="820fd-237">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="820fd-239">Hello alkalmazások listában válassza ki a **Google Apps**.</span><span class="sxs-lookup"><span data-stu-id="820fd-239">In hello applications list, select **Google Apps**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_app.png) 

3. <span data-ttu-id="820fd-241">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="820fd-241">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="820fd-243">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="820fd-243">Click **Add** button.</span></span> <span data-ttu-id="820fd-244">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="820fd-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="820fd-246">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="820fd-246">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="820fd-247">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="820fd-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="820fd-248">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="820fd-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="820fd-249">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="820fd-249">Testing single sign-on</span></span>

<span data-ttu-id="820fd-250">Ebben a szakaszban a egyszeri bejelentkezés beállításokat, nyissa meg a hozzáférési panelre a hello tootest [https://myapps.microsoft.com](active-directory-saas-access-panel-introduction.md), majd bejelentkezés a hello teszt fiókba, és kattintson **Google Apps** hello hozzáférési Panel csempére.</span><span class="sxs-lookup"><span data-stu-id="820fd-250">In this section, tootest your single sign-on settings, open hello Access Panel at [https://myapps.microsoft.com](active-directory-saas-access-panel-introduction.md), then sign into hello test account, and click **Google Apps** tile in hello Access Panel.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="820fd-251">További források</span><span class="sxs-lookup"><span data-stu-id="820fd-251">Additional resources</span></span>

* [<span data-ttu-id="820fd-252">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="820fd-252">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="820fd-253">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="820fd-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="820fd-254">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="820fd-254">Configure User Provisioning</span></span>](active-directory-saas-google-apps-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_203.png

[0]: ./media/active-directory-saas-google-apps-tutorial/azure-active-directory.png

[5]: ./media/active-directory-saas-google-apps-tutorial/gapps-added.png
[6]: ./media/active-directory-saas-google-apps-tutorial/config-sso.png
[7]: ./media/active-directory-saas-google-apps-tutorial/sso-gapps.png
[8]: ./media/active-directory-saas-google-apps-tutorial/sso-url.png
[9]: ./media/active-directory-saas-google-apps-tutorial/download-cert.png
[10]: ./media/active-directory-saas-google-apps-tutorial/gapps-security.png
[11]: ./media/active-directory-saas-google-apps-tutorial/security-gapps.png
[12]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-config.png
[13]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-confirm.png
[14]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-email.png
[15]: ./media/active-directory-saas-google-apps-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-tutorial/gapps-api-enabled.png
[17]: ./media/active-directory-saas-google-apps-tutorial/add-custom-domain.png
[18]: ./media/active-directory-saas-google-apps-tutorial/specify-domain.png
[19]: ./media/active-directory-saas-google-apps-tutorial/verify-domain.png
[20]: ./media/active-directory-saas-google-apps-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-another.png
[23]: ./media/active-directory-saas-google-apps-tutorial/apps-gapps.png
[24]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-tutorial/gapps-auth.png
[29]: ./media/active-directory-saas-google-apps-tutorial/assign-users.png
[30]: ./media/active-directory-saas-google-apps-tutorial/assign-confirm.png