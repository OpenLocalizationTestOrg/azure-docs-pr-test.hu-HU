---
title: "Oktatóanyag: Azure Active Directory-integráció a Google Apps, az Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Google alkalmazások között."
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
ms.openlocfilehash: 065841d6b4fe50e953f01bba4d3f23de82b82726
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-google-apps"></a><span data-ttu-id="12c6c-103">Oktatóanyag: Azure Active Directory-integráció a Google Apps</span><span class="sxs-lookup"><span data-stu-id="12c6c-103">Tutorial: Azure Active Directory integration with Google Apps</span></span>

<span data-ttu-id="12c6c-104">Ebben az oktatóanyagban elsajátíthatja Google alkalmazások integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="12c6c-104">In this tutorial, you learn how to integrate Google Apps with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="12c6c-105">Google Apps alkalmazások integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="12c6c-105">Integrating Google Apps with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="12c6c-106">Megadhatja a Google Apps hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="12c6c-106">You can control in Azure AD who has access to Google Apps</span></span>
- <span data-ttu-id="12c6c-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Google Apps (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="12c6c-107">You can enable your users to automatically get signed-on to Google Apps (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="12c6c-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="12c6c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="12c6c-109">Ha szeretné tudni, hogy az Azure AD SaaS integrálásáról további információt, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="12c6c-109">If you want to know more information about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12c6c-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="12c6c-110">Prerequisites</span></span>

<span data-ttu-id="12c6c-111">Az Azure AD-integráció konfigurálása a Google Apps, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="12c6c-111">To configure Azure AD integration with Google Apps, you need the following items:</span></span>

- <span data-ttu-id="12c6c-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="12c6c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="12c6c-113">A Google Apps egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="12c6c-113">A Google Apps single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="12c6c-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="12c6c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="12c6c-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="12c6c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="12c6c-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="12c6c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="12c6c-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="12c6c-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="video-tutorial"></a><span data-ttu-id="12c6c-118">Oktatóvideó</span><span class="sxs-lookup"><span data-stu-id="12c6c-118">Video tutorial</span></span>
<span data-ttu-id="12c6c-119">Útmutató 2 percet az egyszeri bejelentkezés Google alkalmazások engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="12c6c-119">How to Enable Single Sign-On to Google Apps in 2 Minutes:</span></span>

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enable-single-sign-on-to-Google-Apps-in-2-minutes-with-Azure-AD/player]

## <a name="frequently-asked-questions"></a><span data-ttu-id="12c6c-120">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="12c6c-120">Frequently Asked Questions</span></span>
1. <span data-ttu-id="12c6c-121">**K: Chromebooks és egyéb Chrome eszközök kompatibilisek legyenek az Azure AD az egyszeri bejelentkezés?**</span><span class="sxs-lookup"><span data-stu-id="12c6c-121">**Q: Are Chromebooks and other Chrome devices compatible with Azure AD single sign-on?**</span></span>
   
    <span data-ttu-id="12c6c-122">V: Igen, a felhasználók jelentkezhetnek be a Azure AD hitelesítő adataik használatával Chromebook eszközeiket.</span><span class="sxs-lookup"><span data-stu-id="12c6c-122">A: Yes, users are able to sign into their Chromebook devices using their Azure AD credentials.</span></span> <span data-ttu-id="12c6c-123">Ez [Google alkalmazások támogatják a cikk](https://support.google.com/chrome/a/answer/6060880) információt arról, hogy miért felhasználók lehet kérni a hitelesítő adatok kétszer.</span><span class="sxs-lookup"><span data-stu-id="12c6c-123">See this [Google Apps support article](https://support.google.com/chrome/a/answer/6060880) for information on why users may get prompted for credentials twice.</span></span>

2. <span data-ttu-id="12c6c-124">**Kérdés: Ha az egyszeri bejelentkezés engedélyezése felhasználók fogja tudni az Azure AD hitelesítő adatai segítségével jelentkezzen be Google bármely termék, például a Google osztályteremben, GMail, Google meghajtó, YouTube stb?**</span><span class="sxs-lookup"><span data-stu-id="12c6c-124">**Q: If I enable single sign-on, will users be able to use their Azure AD credentials to sign into any Google product, such as Google Classroom, GMail, Google Drive, YouTube, and so on?**</span></span>
   
    <span data-ttu-id="12c6c-125">V: Igen, attól függően [mely Google apps](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) úgy dönt, hogy engedélyezi vagy letiltja a szervezet számára.</span><span class="sxs-lookup"><span data-stu-id="12c6c-125">A: Yes, depending on [which Google apps](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) you choose to enable or disable for your organization.</span></span>

3. <span data-ttu-id="12c6c-126">**K: engedélyezhető az egyszeri bejelentkezéshez csak egy része a Google Apps-alkalmazások felhasználók számára?**</span><span class="sxs-lookup"><span data-stu-id="12c6c-126">**Q: Can I enable single sign-on for only a subset of my Google Apps users?**</span></span>
   
    <span data-ttu-id="12c6c-127">A: nem bekapcsolásával egyszeri bejelentkezés azonnal minden a Google Apps felhasználótól megköveteli a hitelesítést az Azure AD hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="12c6c-127">A: No, turning on single sign-on immediately requires all your Google Apps users to authenticate with their Azure AD credentials.</span></span> <span data-ttu-id="12c6c-128">Google Apps nem támogatja több identitás-szolgáltatóktól rendelkező, mert az identitásszolgáltató a Google Apps környezetnek lehet az Azure AD vagy Google –, de nem mindkettőt egyszerre.</span><span class="sxs-lookup"><span data-stu-id="12c6c-128">Because Google Apps doesn't support having multiple identity providers, the identity provider for your Google Apps environment can either be Azure AD or Google -- but not both at the same time.</span></span>

4. <span data-ttu-id="12c6c-129">**K:, ha van bejelentkezett felhasználó Windows keresztül, azok automatikusan hitelesíteniük kell Google Apps első kéri a jelszót nélkül?**</span><span class="sxs-lookup"><span data-stu-id="12c6c-129">**Q: If a user is signed in through Windows, are they automatically authenticate to Google Apps without getting prompted for a password?**</span></span>
   
    <span data-ttu-id="12c6c-130">A: Ez a forgatókönyv engedélyezésének két lehetőség áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="12c6c-130">A: There are two options for enabling this scenario.</span></span> <span data-ttu-id="12c6c-131">Először sikerült jelentkeznek be Windows 10-eszközöket az [Azure Active Directory csatlakozási](active-directory-azureadjoin-overview.md).</span><span class="sxs-lookup"><span data-stu-id="12c6c-131">First, users could sign into Windows 10 devices via [Azure Active Directory Join](active-directory-azureadjoin-overview.md).</span></span> <span data-ttu-id="12c6c-132">Azt is megteheti, sikerült jelentkeznek be Windows-eszközök, amelyek a tartományhoz, amely az egyszeri bejelentkezés az Azure AD-keresztül engedélyezve van a helyszíni Active Directory egy [Active Directory összevonási szolgáltatások (AD FS)](active-directory-aadconnect-user-signin.md) központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="12c6c-132">Alternatively, users could sign into Windows devices that are domain-joined to an on-premises Active Directory that has been enabled for single sign-on to Azure AD via an [Active Directory Federation Services (AD FS)](active-directory-aadconnect-user-signin.md) deployment.</span></span> <span data-ttu-id="12c6c-133">Mindkét lehetőség kell ahhoz, hogy az egyszeri bejelentkezés az Azure AD között az alábbi oktatóanyag hajtsa végre a lépéseket és a Google Apps.</span><span class="sxs-lookup"><span data-stu-id="12c6c-133">Both options require you to perform the steps in the following tutorial to enable single sign-on between Azure AD and Google Apps.</span></span>

## <a name="scenario-description"></a><span data-ttu-id="12c6c-134">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="12c6c-134">Scenario description</span></span>
<span data-ttu-id="12c6c-135">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="12c6c-135">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="12c6c-136">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="12c6c-136">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="12c6c-137">Google Apps hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="12c6c-137">Adding Google Apps from the gallery</span></span>
2. <span data-ttu-id="12c6c-138">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="12c6c-138">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-google-apps-from-the-gallery"></a><span data-ttu-id="12c6c-139">Google Apps hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="12c6c-139">Adding Google Apps from the gallery</span></span>
<span data-ttu-id="12c6c-140">Az Azure AD integrálása a Google Apps konfigurálásához kell hozzáadnia Google Apps a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="12c6c-140">To configure the integration of Google Apps into Azure AD, you need to add Google Apps from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="12c6c-141">**Adja hozzá a Google Apps a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="12c6c-141">**To add Google Apps from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="12c6c-142">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="12c6c-142">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="12c6c-144">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="12c6c-144">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="12c6c-145">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="12c6c-145">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="12c6c-147">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="12c6c-147">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="12c6c-149">Írja be a keresőmezőbe, **Google Apps**.</span><span class="sxs-lookup"><span data-stu-id="12c6c-149">In the search box, type **Google Apps**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_search.png)

5. <span data-ttu-id="12c6c-151">Az eredmények panelen válassza ki a **Google Apps**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="12c6c-151">In the results panel, select **Google Apps**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="12c6c-153">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="12c6c-153">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="12c6c-154">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Google Apps "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="12c6c-154">In this section, you configure and test Azure AD single sign-on with Google Apps based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="12c6c-155">Az egyszeri bejelentkezés működéséhez az Azure AD számára a Google Apps a partner felhasználót egy felhasználó számára az Azure AD kell.</span><span class="sxs-lookup"><span data-stu-id="12c6c-155">For single sign-on to work, Azure AD needs to know what the counterpart user in Google Apps is to a user in Azure AD.</span></span> <span data-ttu-id="12c6c-156">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Google Apps közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="12c6c-156">In other words, a link relationship between an Azure AD user and the related user in Google Apps needs to be established.</span></span>

<span data-ttu-id="12c6c-157">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a Google Apps.</span><span class="sxs-lookup"><span data-stu-id="12c6c-157">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Google Apps.</span></span>

<span data-ttu-id="12c6c-158">Az Azure AD az egyszeri bejelentkezés Google alkalmazások tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="12c6c-158">To configure and test Azure AD single sign-on with Google Apps, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="12c6c-159">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="12c6c-159">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="12c6c-160">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="12c6c-160">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="12c6c-161">**[Google Apps tesztfelhasználó létrehozása](#creating-a-google-apps-test-user)**  - való Britta Simon egy megfelelője a Google Apps, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="12c6c-161">**[Creating a Google Apps test user](#creating-a-google-apps-test-user)** - to have a counterpart of Britta Simon in Google Apps that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="12c6c-162">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="12c6c-162">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="12c6c-163">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="12c6c-163">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="12c6c-164">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="12c6c-164">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="12c6c-165">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Google Apps-alkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="12c6c-165">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Google Apps application.</span></span>

<span data-ttu-id="12c6c-166">**Konfigurálja az Azure AD egyszeri bejelentkezést a Google Apps, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="12c6c-166">**To configure Azure AD single sign-on with Google Apps, perform the following steps:**</span></span>

1. <span data-ttu-id="12c6c-167">Az Azure portálon a a **Google Apps** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="12c6c-167">In the Azure portal, on the **Google Apps** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="12c6c-169">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="12c6c-169">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_samlbase.png)

3. <span data-ttu-id="12c6c-171">Az a **Google Apps tartományi és URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="12c6c-171">On the **Google Apps Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_url.png)

    <span data-ttu-id="12c6c-173">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://mail.google.com/a/<yourdomain>`</span><span class="sxs-lookup"><span data-stu-id="12c6c-173">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://mail.google.com/a/<yourdomain>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="12c6c-174">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="12c6c-174">This value is not real.</span></span> <span data-ttu-id="12c6c-175">Frissítse az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="12c6c-175">Update the value with the actual Sign-on URL.</span></span> <span data-ttu-id="12c6c-176">Lépjen kapcsolatba a [Google támogatási csoport](https://www.google.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="12c6c-176">contact the [Google support team](https://www.google.com/contact/).</span></span>
 
4. <span data-ttu-id="12c6c-177">Az a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány** , és mentse a tanúsítványt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="12c6c-177">On the **SAML Signing Certificate** section, click **Certificate** and then save the certificate on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_certificate.png) 

5. <span data-ttu-id="12c6c-179">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="12c6c-179">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-google-apps-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="12c6c-181">Az a **Google Apps konfigurációs** területen kattintson **konfigurálása Google Apps** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="12c6c-181">On the **Google Apps Configuration** section, click **Configure Google Apps** to open **Configure sign-on** window.</span></span> <span data-ttu-id="12c6c-182">Másolás a **Sign-Out URL-címet, a SAML-alapú egyszeri bejelentkezési URL-címe és a módosítás jelszó URL-cím** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="12c6c-182">Copy the **Sign-Out URL, SAML Single Sign-On Service URL and Change password URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_configure.png) 

7. <span data-ttu-id="12c6c-184">Új lap megnyitása a böngészőben, és jelentkezzen be a [Google Apps felügyeleti konzol](http://admin.google.com/) a rendszergazdai fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="12c6c-184">Open a new tab in your browser, and sign into the [Google Apps Admin Console](http://admin.google.com/) using your administrator account.</span></span>

8. <span data-ttu-id="12c6c-185">Kattintson a **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="12c6c-185">Click **Security**.</span></span> <span data-ttu-id="12c6c-186">Ha nem látja a hivatkozásra, akkor előfordulhat, hogy rejtve alatt a **további vezérlők** menü a képernyő alján.</span><span class="sxs-lookup"><span data-stu-id="12c6c-186">If you don't see the link, it may be hidden under the **More Controls** menu at the bottom of the screen.</span></span>
   
    ![Kattintson a Security (Biztonság) elemre.][10]

9. <span data-ttu-id="12c6c-188">Az a **biztonsági** kattintson **beállítása az egyszeri bejelentkezés (SSO).**</span><span class="sxs-lookup"><span data-stu-id="12c6c-188">On the **Security** page, click **Set up single sign-on (SSO).**</span></span>
   
    ![Kattintson az egyszeri Bejelentkezést.][11]

10. <span data-ttu-id="12c6c-190">Hajtsa végre a következő konfigurációs módosításokat:</span><span class="sxs-lookup"><span data-stu-id="12c6c-190">Perform the following configuration changes:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][12]
   
    <span data-ttu-id="12c6c-192">a.</span><span class="sxs-lookup"><span data-stu-id="12c6c-192">a.</span></span> <span data-ttu-id="12c6c-193">Válassza ki **külső identitásszolgáltatótól telepítő SSO**.</span><span class="sxs-lookup"><span data-stu-id="12c6c-193">Select **Setup SSO with third-party identity provider**.</span></span>

    <span data-ttu-id="12c6c-194">b.</span><span class="sxs-lookup"><span data-stu-id="12c6c-194">b.</span></span> <span data-ttu-id="12c6c-195">A a **bejelentkezési URL-címe** Google Apps mezőbe illessze be az értékét **egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="12c6c-195">In the **Sign-in page URL** field in Google Apps, paste the value of **Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="12c6c-196">c.</span><span class="sxs-lookup"><span data-stu-id="12c6c-196">c.</span></span> <span data-ttu-id="12c6c-197">Az a **kijelentkezési URL-címe** Google Apps mezőbe illessze be az értékét **Sign-Out URL-cím**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="12c6c-197">In the **Sign-out page URL** field in Google Apps, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="12c6c-198">d.</span><span class="sxs-lookup"><span data-stu-id="12c6c-198">d.</span></span> <span data-ttu-id="12c6c-199">Az a **Módosítsa jelszavát URL-címet** Google Apps mezőbe illessze be az értékét **Módosítsa jelszavát URL-címet**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="12c6c-199">In the **Change password URL** field in Google Apps, paste the value of **Change password URL**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="12c6c-200">e.</span><span class="sxs-lookup"><span data-stu-id="12c6c-200">e.</span></span> <span data-ttu-id="12c6c-201">A Google Apps az a **ellenőrző tanúsítvány**, az Azure-portálról letöltött tanúsítvány feltöltése.</span><span class="sxs-lookup"><span data-stu-id="12c6c-201">In Google Apps, for the **Verification certificate**, upload the certificate that you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="12c6c-202">f.</span><span class="sxs-lookup"><span data-stu-id="12c6c-202">f.</span></span> <span data-ttu-id="12c6c-203">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="12c6c-203">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="12c6c-204">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="12c6c-204">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="12c6c-205">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="12c6c-205">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="12c6c-206">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="12c6c-206">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="12c6c-207">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="12c6c-207">Creating an Azure AD test user</span></span>
<span data-ttu-id="12c6c-208">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="12c6c-208">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="12c6c-210">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="12c6c-210">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="12c6c-211">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="12c6c-211">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-google-apps-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="12c6c-213">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="12c6c-213">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-google-apps-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="12c6c-215">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="12c6c-215">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-google-apps-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="12c6c-217">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="12c6c-217">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-google-apps-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="12c6c-219">a.</span><span class="sxs-lookup"><span data-stu-id="12c6c-219">a.</span></span> <span data-ttu-id="12c6c-220">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="12c6c-220">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="12c6c-221">b.</span><span class="sxs-lookup"><span data-stu-id="12c6c-221">b.</span></span> <span data-ttu-id="12c6c-222">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="12c6c-222">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="12c6c-223">c.</span><span class="sxs-lookup"><span data-stu-id="12c6c-223">c.</span></span> <span data-ttu-id="12c6c-224">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="12c6c-224">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="12c6c-225">d.</span><span class="sxs-lookup"><span data-stu-id="12c6c-225">d.</span></span> <span data-ttu-id="12c6c-226">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="12c6c-226">Click **Create**.</span></span>
 
### <a name="creating-a-google-apps-test-user"></a><span data-ttu-id="12c6c-227">Google Apps tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="12c6c-227">Creating a Google Apps test user</span></span>

<span data-ttu-id="12c6c-228">Ez a szakasz célja a Google Apps szoftver Britta Simon nevű felhasználót kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="12c6c-228">The objective of this section is to create a user called Britta Simon in Google Apps Software.</span></span> <span data-ttu-id="12c6c-229">Google Apps támogatja az Automatikus kiépítés, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="12c6c-229">Google Apps supports auto provisioning, which is by default enabled.</span></span> <span data-ttu-id="12c6c-230">Nincs olyan művelet, ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="12c6c-230">There is no action for you in this section.</span></span> <span data-ttu-id="12c6c-231">Ha a felhasználó nem létezik a Google Apps szoftver, egy új jön létre, Google Apps szoftver elérésére tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="12c6c-231">If a user doesn't already exist in Google Apps Software, a new one is created when you attempt to access Google Apps Software.</span></span>

>[!NOTE] 
><span data-ttu-id="12c6c-232">Ha a felhasználó manuális létrehozása, forduljon a [Google támogatási csoport](https://www.google.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="12c6c-232">If you need to create a user manually, contact the [Google support team](https://www.google.com/contact/).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="12c6c-233">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="12c6c-233">Assigning the Azure AD test user</span></span>

<span data-ttu-id="12c6c-234">Ebben a szakaszban Britta Simon hozzáférést biztosít a Google Apps által használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="12c6c-234">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Google Apps.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="12c6c-236">**Google Apps Britta Simon rendel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="12c6c-236">**To assign Britta Simon to Google Apps, perform the following steps:**</span></span>

1. <span data-ttu-id="12c6c-237">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="12c6c-237">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="12c6c-239">Az alkalmazások listában válassza ki a **Google Apps**.</span><span class="sxs-lookup"><span data-stu-id="12c6c-239">In the applications list, select **Google Apps**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_app.png) 

3. <span data-ttu-id="12c6c-241">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="12c6c-241">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="12c6c-243">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="12c6c-243">Click **Add** button.</span></span> <span data-ttu-id="12c6c-244">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="12c6c-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="12c6c-246">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="12c6c-246">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="12c6c-247">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="12c6c-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="12c6c-248">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="12c6c-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="12c6c-249">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="12c6c-249">Testing single sign-on</span></span>

<span data-ttu-id="12c6c-250">Ez a szakasz az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési panelre a [https://myapps.microsoft.com](active-directory-saas-access-panel-introduction.md), majd jelentkezzen be a fiókot, és kattintson a **Google Apps** csempére a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="12c6c-250">In this section, to test your single sign-on settings, open the Access Panel at [https://myapps.microsoft.com](active-directory-saas-access-panel-introduction.md), then sign into the test account, and click **Google Apps** tile in the Access Panel.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="12c6c-251">További források</span><span class="sxs-lookup"><span data-stu-id="12c6c-251">Additional resources</span></span>

* [<span data-ttu-id="12c6c-252">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="12c6c-252">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="12c6c-253">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="12c6c-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="12c6c-254">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="12c6c-254">Configure User Provisioning</span></span>](active-directory-saas-google-apps-provisioning-tutorial.md)

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