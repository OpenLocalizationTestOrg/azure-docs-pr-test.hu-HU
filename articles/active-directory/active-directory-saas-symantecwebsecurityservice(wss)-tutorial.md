---
title: "Oktatóanyag: Azure Active Directory-integráció Symantec webes biztonsági szolgáltatás (VSS) |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Symantec webes biztonsági szolgáltatás (VSS) között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: d0738bb750a82863b2f77540e8e7b94cca4b64e3
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a><span data-ttu-id="14f1d-103">Oktatóanyag: Azure Active Directory-integráció Symantec webes biztonsági szolgáltatás (VSS)</span><span class="sxs-lookup"><span data-stu-id="14f1d-103">Tutorial: Azure Active Directory integration with Symantec Web Security Service (WSS)</span></span>

<span data-ttu-id="14f1d-104">Ebben az oktatóanyagban elsajátíthatja Symantec webes biztonsági szolgáltatás (VSS) integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="14f1d-104">In this tutorial, you learn how to integrate Symantec Web Security Service (WSS) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="14f1d-105">Symantec webes biztonsági szolgáltatás (VSS) integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="14f1d-105">Integrating Symantec Web Security Service (WSS) with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="14f1d-106">Szabályozhatja, aki hozzáfér Symantec webes biztonsági szolgáltatás (VSS) Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="14f1d-106">You can control in Azure AD who has access to Symantec Web Security Service (WSS)</span></span>
- <span data-ttu-id="14f1d-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett a Symantec webes biztonsági szolgáltatás (VSS) (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="14f1d-107">You can enable your users to automatically get signed-on to Symantec Web Security Service (WSS) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="14f1d-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="14f1d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="14f1d-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="14f1d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="14f1d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="14f1d-110">Prerequisites</span></span>

<span data-ttu-id="14f1d-111">Az Azure AD-integráció konfigurálása a Symantec webes biztonsági szolgáltatás (VSS), a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="14f1d-111">To configure Azure AD integration with Symantec Web Security Service (WSS), you need the following items:</span></span>

- <span data-ttu-id="14f1d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="14f1d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="14f1d-113">A Symantec webes biztonsági szolgáltatás (VSS) egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="14f1d-113">A Symantec Web Security Service (WSS) single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="14f1d-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="14f1d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="14f1d-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="14f1d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="14f1d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="14f1d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="14f1d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="14f1d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="14f1d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="14f1d-118">Scenario description</span></span>
<span data-ttu-id="14f1d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="14f1d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="14f1d-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="14f1d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="14f1d-121">Symantec webes biztonsági szolgáltatás (VSS) hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="14f1d-121">Adding Symantec Web Security Service (WSS) from the gallery</span></span>
2. <span data-ttu-id="14f1d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="14f1d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-symantec-web-security-service-wss-from-the-gallery"></a><span data-ttu-id="14f1d-123">Symantec webes biztonsági szolgáltatás (VSS) hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="14f1d-123">Adding Symantec Web Security Service (WSS) from the gallery</span></span>
<span data-ttu-id="14f1d-124">Az Azure AD integrálása Symantec webes biztonsági szolgáltatás (VSS) konfigurálásához kell hozzáadnia Symantec webes biztonsági szolgáltatás (VSS) a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="14f1d-124">To configure the integration of Symantec Web Security Service (WSS) into Azure AD, you need to add Symantec Web Security Service (WSS) from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="14f1d-125">**Adja hozzá a Symantec webes biztonsági szolgáltatás (VSS) a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="14f1d-125">**To add Symantec Web Security Service (WSS) from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="14f1d-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="14f1d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="14f1d-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="14f1d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="14f1d-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="14f1d-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="14f1d-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="14f1d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="14f1d-133">Írja be a keresőmezőbe, **Symantec webes biztonsági szolgáltatás (VSS)**.</span><span class="sxs-lookup"><span data-stu-id="14f1d-133">In the search box, type **Symantec Web Security Service (WSS)**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_search.png)

5. <span data-ttu-id="14f1d-135">Az eredmények panelen válassza ki a **Symantec webes biztonsági szolgáltatás (VSS)**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="14f1d-135">In the results panel, select **Symantec Web Security Service (WSS)**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="14f1d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="14f1d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="14f1d-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Symantec webes biztonsági szolgáltatás (WSS) "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="14f1d-138">In this section, you configure and test Azure AD single sign-on with Symantec Web Security Service (WSS) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="14f1d-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Symantec webes biztonsági szolgáltatás (VSS) a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="14f1d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Symantec Web Security Service (WSS) is to a user in Azure AD.</span></span> <span data-ttu-id="14f1d-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Symantec webes biztonsági szolgáltatás (VSS) közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="14f1d-140">In other words, a link relationship between an Azure AD user and the related user in Symantec Web Security Service (WSS) needs to be established.</span></span>

<span data-ttu-id="14f1d-141">A Symantec webes biztonsági szolgáltatás (VSS), rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="14f1d-141">In Symantec Web Security Service (WSS), assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="14f1d-142">Az Azure AD az egyszeri bejelentkezés Symantec webes biztonsági szolgáltatás (VSS) tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="14f1d-142">To configure and test Azure AD single sign-on with Symantec Web Security Service (WSS), you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="14f1d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="14f1d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="14f1d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="14f1d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="14f1d-145">**[Symantec webes biztonsági szolgáltatás (VSS) tesztfelhasználó létrehozása](#creating-a-symantec-web-security-service-wss-test-user)**  - kell rendelkeznie a megfelelője a Britta Simon a Symantec webes biztonsági szolgáltatás (WSS), amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="14f1d-145">**[Creating a Symantec Web Security Service (WSS) test user](#creating-a-symantec-web-security-service-wss-test-user)** - to have a counterpart of Britta Simon in Symantec Web Security Service (WSS) that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="14f1d-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="14f1d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="14f1d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="14f1d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="14f1d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="14f1d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="14f1d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Symantec webes biztonsági szolgáltatás (VSS) alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="14f1d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Symantec Web Security Service (WSS) application.</span></span>

<span data-ttu-id="14f1d-150">**Symantec webes biztonsági szolgáltatás (VSS) konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="14f1d-150">**To configure Azure AD single sign-on with Symantec Web Security Service (WSS), perform the following steps:**</span></span>

1. <span data-ttu-id="14f1d-151">Az Azure portálon a a **Symantec webes biztonsági szolgáltatás (VSS)** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="14f1d-151">In the Azure portal, on the **Symantec Web Security Service (WSS)** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="14f1d-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="14f1d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

3. <span data-ttu-id="14f1d-155">Az a **Symantec webes biztonsági szolgáltatás (VSS) tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="14f1d-155">On the **Symantec Web Security Service (WSS) Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    <span data-ttu-id="14f1d-157">a.</span><span class="sxs-lookup"><span data-stu-id="14f1d-157">a.</span></span> <span data-ttu-id="14f1d-158">Az a **bejelentkezési URL-cím** szövegmezőhöz említse meg az URL-cím, amelynek meg szeretné akadályozni a blokkolási üzletszabályzata előírja a Symantec webes biztonsági szolgáltatás (VSS).</span><span class="sxs-lookup"><span data-stu-id="14f1d-158">In the **Sign-on URL** textbox, mention the url, which you want to block according to the blocking policy of the Symantec Web Security Service (WSS).</span></span>  
    
    <span data-ttu-id="14f1d-159">b.</span><span class="sxs-lookup"><span data-stu-id="14f1d-159">b.</span></span> <span data-ttu-id="14f1d-160">Az a **azonosító** szövegmező, írja be az URL-cím:`https://saml.threatpulse.net:8443/saml/saml_realm`</span><span class="sxs-lookup"><span data-stu-id="14f1d-160">In the **Identifier** textbox, type the URL: `https://saml.threatpulse.net:8443/saml/saml_realm`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="14f1d-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="14f1d-161">These values are not real.</span></span> <span data-ttu-id="14f1d-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="14f1d-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="14f1d-163">Ügyfél [Symantec webes biztonsági szolgáltatás (VSS) ügyfél-támogatási csoport](https://www.symantec.com/contact-us) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="14f1d-163">Contact [Symantec Web Security Service (WSS) Client support team](https://www.symantec.com/contact-us) to get these values.</span></span> 

4. <span data-ttu-id="14f1d-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="14f1d-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

5. <span data-ttu-id="14f1d-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="14f1d-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="14f1d-168">Egyszeri bejelentkezés konfigurálása **Symantec webes biztonsági szolgáltatás (VSS)** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [Symantec webes biztonsági szolgáltatás (VSS) támogatási csoport](https://www.symantec.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="14f1d-168">To configure single sign-on on **Symantec Web Security Service (WSS)** side, you need to send the downloaded **Metadata XML** to [Symantec Web Security Service (WSS) support team](https://www.symantec.com/contact-us).</span></span>

> [!TIP]
> <span data-ttu-id="14f1d-169">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="14f1d-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="14f1d-170">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="14f1d-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="14f1d-171">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="14f1d-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="14f1d-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="14f1d-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="14f1d-173">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="14f1d-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="14f1d-175">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="14f1d-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="14f1d-176">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="14f1d-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="14f1d-178">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="14f1d-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="14f1d-180">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="14f1d-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="14f1d-182">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="14f1d-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="14f1d-184">a.</span><span class="sxs-lookup"><span data-stu-id="14f1d-184">a.</span></span> <span data-ttu-id="14f1d-185">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="14f1d-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="14f1d-186">b.</span><span class="sxs-lookup"><span data-stu-id="14f1d-186">b.</span></span> <span data-ttu-id="14f1d-187">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="14f1d-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="14f1d-188">c.</span><span class="sxs-lookup"><span data-stu-id="14f1d-188">c.</span></span> <span data-ttu-id="14f1d-189">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="14f1d-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="14f1d-190">d.</span><span class="sxs-lookup"><span data-stu-id="14f1d-190">d.</span></span> <span data-ttu-id="14f1d-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="14f1d-191">Click **Create**.</span></span>
 
### <a name="creating-a-symantec-web-security-service-wss-test-user"></a><span data-ttu-id="14f1d-192">Symantec webes biztonsági szolgáltatás (VSS) tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="14f1d-192">Creating a Symantec Web Security Service (WSS) test user</span></span>

<span data-ttu-id="14f1d-193">Ebben a szakaszban nevű Britta Simon Symantec webes biztonsági szolgáltatás (VSS) a felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="14f1d-193">In this section, you create a user called Britta Simon in Symantec Web Security Service (WSS).</span></span> <span data-ttu-id="14f1d-194">Együttműködve [Symantec webes biztonsági szolgáltatás (VSS) támogatási csoport](https://www.symantec.com/contact-us) a felhasználók hozzáadása a Symantec webes biztonsági szolgáltatás (VSS) platform.</span><span class="sxs-lookup"><span data-stu-id="14f1d-194">Work with [Symantec Web Security Service (WSS) support team](https://www.symantec.com/contact-us) to add the users in the Symantec Web Security Service (WSS) platform.</span></span> <span data-ttu-id="14f1d-195">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="14f1d-195">Users must be created and activated before you use single sign-on.</span></span> <span data-ttu-id="14f1d-196">Is együtt a felhasználó adatait meg kell küldeni a nyilvános IP-cím a gép, amelyről próbál hozzáférni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="14f1d-196">Also along with the user details you need to send the public IPaddress of the machine from which you are trying to access the application.</span></span>

> [!NOTE]
> <span data-ttu-id="14f1d-197">Adjon [ide](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) beolvasni a gépek tartozó nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="14f1d-197">Please [click here](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) to get your machine's public IPaddress.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="14f1d-198">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="14f1d-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="14f1d-199">Ebben a szakaszban engedélyezze Britta Simon Symantec webes biztonsági szolgáltatás (VSS) nyújtó Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="14f1d-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Symantec Web Security Service (WSS).</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="14f1d-201">**Symantec webes biztonsági szolgáltatás (VSS) Britta Simon hozzárendeléséhez a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="14f1d-201">**To assign Britta Simon to Symantec Web Security Service (WSS), perform the following steps:**</span></span>

1. <span data-ttu-id="14f1d-202">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="14f1d-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="14f1d-204">Az alkalmazások listában válassza ki a **Symantec webes biztonsági szolgáltatás (VSS)**.</span><span class="sxs-lookup"><span data-stu-id="14f1d-204">In the applications list, select **Symantec Web Security Service (WSS)**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_app.png) 

3. <span data-ttu-id="14f1d-206">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="14f1d-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="14f1d-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="14f1d-208">Click **Add** button.</span></span> <span data-ttu-id="14f1d-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="14f1d-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="14f1d-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="14f1d-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="14f1d-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="14f1d-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="14f1d-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="14f1d-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="14f1d-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="14f1d-214">Testing single sign-on</span></span>

<span data-ttu-id="14f1d-215">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="14f1d-215">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="14f1d-216">A hozzáférési panelen Symantec webes biztonsági szolgáltatás (VSS) csempére kattintva átirányítja a blokkolási oldalra, amelynek a letiltó házirendet konfiguráltak.</span><span class="sxs-lookup"><span data-stu-id="14f1d-216">When you click the Symantec Web Security Service (WSS) tile in the Access Panel, you get redirected to the blocking page for which the blocking policy has been configured.</span></span>
<span data-ttu-id="14f1d-217">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="14f1d-217">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="14f1d-218">További források</span><span class="sxs-lookup"><span data-stu-id="14f1d-218">Additional resources</span></span>

* [<span data-ttu-id="14f1d-219">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="14f1d-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="14f1d-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="14f1d-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_203.png

