---
title: "Oktatóanyag: Azure Active Directory-integráció Symantec webes biztonsági szolgáltatás (VSS) |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Symantec webes biztonsági szolgáltatás (VSS) között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 61576d3a915d209e7355e04432e586dcf66e7c5a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a><span data-ttu-id="c8979-103">Oktatóanyag: Azure Active Directory-integráció Symantec webes biztonsági szolgáltatás (VSS)</span><span class="sxs-lookup"><span data-stu-id="c8979-103">Tutorial: Azure Active Directory integration with Symantec Web Security Service (WSS)</span></span>

<span data-ttu-id="c8979-104">Ebben az oktatóanyagban elsajátíthatja Symantec webes biztonsági szolgáltatás (VSS) fiókja integrálása az Azure Active Directory (Azure AD-) fiók, így a WSS az Azure AD SAML-alapú hitelesítést használó kiépítve a felhasználó hitelesítésére, és a felhasználó vagy csoport szintű szabályzat szabályok érvényesítése lesz.</span><span class="sxs-lookup"><span data-stu-id="c8979-104">In this tutorial, you will learn how to integrate your Symantec Web Security Service (WSS) account with your Azure Active Directory (Azure AD) account so that WSS can authenticate an end user provisioned in the Azure AD using SAML authentication and enforce user or group level policy rules.</span></span>

<span data-ttu-id="c8979-105">Symantec webes biztonsági szolgáltatás (VSS) integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="c8979-105">Integrating Symantec Web Security Service (WSS) with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c8979-106">Összes felhasználók és csoportok az Azure AD portálon WSS fiókját használják kezelése.</span><span class="sxs-lookup"><span data-stu-id="c8979-106">Manage all of the end users and groups used by your WSS account from your Azure AD portal.</span></span> 

- <span data-ttu-id="c8979-107">Lehetővé teszi a felhasználók hitelesítsék magukat WSS használata az Azure AD hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="c8979-107">Allow the end users to authenticate themselves in WSS using their Azure AD credentials.</span></span>

- <span data-ttu-id="c8979-108">A felhasználó végrehajtásának engedélyezése, és csoportosítsa a WSS-fiókjához megadott szintű szabályzat szabályok.</span><span class="sxs-lookup"><span data-stu-id="c8979-108">Enable the enforcement of user and group level policy rules defined in your WSS account.</span></span>

<span data-ttu-id="c8979-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c8979-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c8979-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c8979-110">Prerequisites</span></span>

<span data-ttu-id="c8979-111">Az Azure AD-integráció konfigurálása a Symantec webes biztonsági szolgáltatás (VSS), a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="c8979-111">To configure Azure AD integration with Symantec Web Security Service (WSS), you need the following items:</span></span>

- <span data-ttu-id="c8979-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c8979-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c8979-113">A Symantec webes biztonsági szolgáltatás (VSS) fiók</span><span class="sxs-lookup"><span data-stu-id="c8979-113">A Symantec Web Security Service (WSS) account</span></span>

> [!NOTE]
> <span data-ttu-id="c8979-114">Ez az oktatóanyag lépéseit teszteléséhez nem javasoljuk a WSS fiókkal, amely jelenleg használatban van az üzemi célra.</span><span class="sxs-lookup"><span data-stu-id="c8979-114">To test the steps in this tutorial, we do not recommend using a WSS account that is currently being used for production purpose.</span></span>

<span data-ttu-id="c8979-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="c8979-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c8979-116">Ne használja a WSS-fiókot, amely jelenleg használja a teszteléshez üzemi célra nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c8979-116">Do not use your WSS account that is currently being used for production purpose for this test unless it is necessary.</span></span>
- <span data-ttu-id="c8979-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c8979-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c8979-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c8979-118">Scenario description</span></span>
<span data-ttu-id="c8979-119">Ebben az oktatóanyagban konfigurálhatja az Azure AD az egyszeri bejelentkezés az Azure AD-fiókot a megadott felhasználói hitelesítő adatok használatával WSS való engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="c8979-119">In this tutorial, you will configure your Azure AD to enable single sign-on to WSS using the end user credentials defined in your Azure AD account.</span></span>
<span data-ttu-id="c8979-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c8979-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c8979-121">A Symantec webes biztonsági szolgáltatás (VSS) alkalmazás hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="c8979-121">Adding the Symantec Web Security Service (WSS) app from the gallery</span></span>
2. <span data-ttu-id="c8979-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c8979-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-symantec-web-security-service-wss-from-the-gallery"></a><span data-ttu-id="c8979-123">Symantec webes biztonsági szolgáltatás (VSS) hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="c8979-123">Adding Symantec Web Security Service (WSS) from the gallery</span></span>
<span data-ttu-id="c8979-124">Az Azure AD integrálása Symantec webes biztonsági szolgáltatás (VSS) konfigurálásához kell hozzáadnia Symantec webes biztonsági szolgáltatás (VSS) a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="c8979-124">To configure the integration of Symantec Web Security Service (WSS) into Azure AD, you need to add Symantec Web Security Service (WSS) from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c8979-125">**Adja hozzá a Symantec webes biztonsági szolgáltatás (VSS) a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c8979-125">**To add Symantec Web Security Service (WSS) from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c8979-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c8979-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="c8979-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c8979-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c8979-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c8979-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="c8979-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="c8979-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="c8979-133">Írja be a keresőmezőbe, **Symantec webes biztonsági szolgáltatás (VSS)**, jelölje be **Symantec webes biztonsági szolgáltatás (VSS)** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c8979-133">In the search box, type **Symantec Web Security Service (WSS)**, select **Symantec Web Security Service (WSS)** from result panel then click **Add** button to add the application.</span></span>

    ![Symantec webes biztonsági szolgáltatás (VSS) az eredménylistában](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="c8979-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c8979-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="c8979-136">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Symantec webes biztonsági szolgáltatás (WSS) "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="c8979-136">In this section, you configure and test Azure AD single sign-on with Symantec Web Security Service (WSS) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c8979-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Symantec webes biztonsági szolgáltatás (VSS) a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="c8979-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Symantec Web Security Service (WSS) is to a user in Azure AD.</span></span> <span data-ttu-id="c8979-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Symantec webes biztonsági szolgáltatás (VSS) közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c8979-138">In other words, a link relationship between an Azure AD user and the related user in Symantec Web Security Service (WSS) needs to be established.</span></span>

<span data-ttu-id="c8979-139">A Symantec webes biztonsági szolgáltatás (VSS), rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="c8979-139">In Symantec Web Security Service (WSS), assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c8979-140">Az Azure AD az egyszeri bejelentkezés Symantec webes biztonsági szolgáltatás (VSS) tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="c8979-140">To configure and test Azure AD single sign-on with Symantec Web Security Service (WSS), you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c8979-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="c8979-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c8979-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="c8979-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c8979-143">**[Symantec webes biztonsági szolgáltatás (VSS) tesztfelhasználó létrehozása](#create-a-symantec-web-security-service-wss-test-user)**  - kell rendelkeznie a megfelelője a Britta Simon a Symantec webes biztonsági szolgáltatás (WSS), amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="c8979-143">**[Create a Symantec Web Security Service (WSS) test user](#create-a-symantec-web-security-service-wss-test-user)** - to have a counterpart of Britta Simon in Symantec Web Security Service (WSS) that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c8979-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c8979-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c8979-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="c8979-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="c8979-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c8979-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="c8979-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Symantec webes biztonsági szolgáltatás (VSS) alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="c8979-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Symantec Web Security Service (WSS) application.</span></span>

<span data-ttu-id="c8979-148">**Symantec webes biztonsági szolgáltatás (VSS) konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c8979-148">**To configure Azure AD single sign-on with Symantec Web Security Service (WSS), perform the following steps:**</span></span>

1. <span data-ttu-id="c8979-149">Az Azure portálon a a **Symantec webes biztonsági szolgáltatás (VSS)** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c8979-149">In the Azure portal, on the **Symantec Web Security Service (WSS)** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="c8979-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c8979-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

3. <span data-ttu-id="c8979-153">Az a **Symantec webes biztonsági szolgáltatás (VSS) tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="c8979-153">On the **Symantec Web Security Service (WSS) Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Symantec webes biztonsági szolgáltatás (VSS) tartományhoz és URL-címek](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    <span data-ttu-id="c8979-155">a.</span><span class="sxs-lookup"><span data-stu-id="c8979-155">a.</span></span> <span data-ttu-id="c8979-156">Az a **azonosító** szövegmező, írja be az URL-cím:`https://saml.threatpulse.net:8443/saml/saml_realm`</span><span class="sxs-lookup"><span data-stu-id="c8979-156">In the **Identifier** textbox, type the URL: `https://saml.threatpulse.net:8443/saml/saml_realm`</span></span>

    <span data-ttu-id="c8979-157">b.</span><span class="sxs-lookup"><span data-stu-id="c8979-157">b.</span></span> <span data-ttu-id="c8979-158">Az a **válasz URL-CÍMEN** szövegmező, írja be az URL-cím:`https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`</span><span class="sxs-lookup"><span data-stu-id="c8979-158">In the **Reply URL** textbox, type the URL: `https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`</span></span>

    > [!NOTE]
    > <span data-ttu-id="c8979-159">Vegye fel a kapcsolatot a [Symantec webes biztonsági szolgáltatás (VSS) ügyfél-támogatási csoport](https://www.symantec.com/contact-us) Ha értékeit a **azonosító** és **válasz URL-CÍMEN** valamilyen okból kifolyólag nem működik.</span><span class="sxs-lookup"><span data-stu-id="c8979-159">Please contact the [Symantec Web Security Service (WSS) Client support team](https://www.symantec.com/contact-us) if the values for the **Identifier** and **Reply URL** are not working for some reason.</span></span>

4. <span data-ttu-id="c8979-160">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c8979-160">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

5. <span data-ttu-id="c8979-162">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8979-162">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-symantec-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="c8979-164">Az egyszeri bejelentkezést a Symantec webes biztonsági szolgáltatás (VSS) oldalon megadásához tekintse meg a WSS online dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="c8979-164">To configure single sign-on on the Symantec Web Security Service (WSS) side, refer to the WSS online documentation.</span></span> <span data-ttu-id="c8979-165">A letöltött **metaadatainak XML-kódja** fájl lehet a WSS-portálon importálni kell.</span><span class="sxs-lookup"><span data-stu-id="c8979-165">The downloaded **Metadata XML** file will need to be imported into the WSS portal.</span></span> <span data-ttu-id="c8979-166">Lépjen kapcsolatba a [Symantec webes biztonsági szolgáltatás (VSS) támogatási csoport](https://www.symantec.com/contact-us) a konfigurációval a WSS-portál segítségért.</span><span class="sxs-lookup"><span data-stu-id="c8979-166">Contact the [Symantec Web Security Service (WSS) support team](https://www.symantec.com/contact-us) if you need assistance with the configuration on the WSS portal.</span></span>

> [!TIP]
> <span data-ttu-id="c8979-167">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="c8979-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c8979-168">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="c8979-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c8979-169">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c8979-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="c8979-170">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="c8979-170">Create an Azure AD test user</span></span>

<span data-ttu-id="c8979-171">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="c8979-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="c8979-173">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c8979-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c8979-174">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8979-174">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-symantec-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="c8979-176">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c8979-176">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-symantec-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="c8979-178">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="c8979-178">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-symantec-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="c8979-180">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c8979-180">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-symantec-tutorial/create_aaduser_04.png)

    <span data-ttu-id="c8979-182">a.</span><span class="sxs-lookup"><span data-stu-id="c8979-182">a.</span></span> <span data-ttu-id="c8979-183">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c8979-183">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c8979-184">b.</span><span class="sxs-lookup"><span data-stu-id="c8979-184">b.</span></span> <span data-ttu-id="c8979-185">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c8979-185">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="c8979-186">c.</span><span class="sxs-lookup"><span data-stu-id="c8979-186">c.</span></span> <span data-ttu-id="c8979-187">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="c8979-187">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="c8979-188">d.</span><span class="sxs-lookup"><span data-stu-id="c8979-188">d.</span></span> <span data-ttu-id="c8979-189">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c8979-189">Click **Create**.</span></span>
 
### <a name="create-a-symantec-web-security-service-wss-test-user"></a><span data-ttu-id="c8979-190">Symantec webes biztonsági szolgáltatás (VSS) tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8979-190">Create a Symantec Web Security Service (WSS) test user</span></span>

<span data-ttu-id="c8979-191">Ebben a szakaszban nevű Britta Simon Symantec webes biztonsági szolgáltatás (VSS) a felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c8979-191">In this section, you create a user called Britta Simon in Symantec Web Security Service (WSS).</span></span> <span data-ttu-id="c8979-192">A megfelelő befejezési felhasználónév manuálisan is létrehozható a WSS-portálon, vagy megvárhatja a felhasználók/csoportok üzembe helyezve (~ 15 perc). néhány perc múlva a WSS-portál szinkronizálásának engedélyezése az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="c8979-192">The corresponding end username can be manually created in the WSS portal or you can wait for the users/groups provisioned in the Azure AD to be synchronized to the WSS portal after a few minutes (~15 minutes).</span></span> <span data-ttu-id="c8979-193">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="c8979-193">Users must be created and activated before you use single sign-on.</span></span> <span data-ttu-id="c8979-194">A nyilvános IP-cím használatával keresse meg a webhelyek végfelhasználó gép is ki kell építeni a Symantec webes biztonsági szolgáltatás (VSS) portálon.</span><span class="sxs-lookup"><span data-stu-id="c8979-194">The public IP address of the end user machine, which will be used to browse websites also need to be provisioned in the Symantec Web Security Service (WSS) portal.</span></span>

> [!NOTE]
> <span data-ttu-id="c8979-195">Adjon [ide](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) beolvasni a gépek tartozó nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="c8979-195">Please [click here](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) to get your machine's public IPaddress.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="c8979-196">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="c8979-196">Assign the Azure AD test user</span></span>

<span data-ttu-id="c8979-197">Ebben a szakaszban engedélyezze Britta Simon Symantec webes biztonsági szolgáltatás (VSS) nyújtó Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="c8979-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Symantec Web Security Service (WSS).</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="c8979-199">**Symantec webes biztonsági szolgáltatás (VSS) Britta Simon hozzárendeléséhez a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="c8979-199">**To assign Britta Simon to Symantec Web Security Service (WSS), perform the following steps:**</span></span>

1. <span data-ttu-id="c8979-200">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c8979-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c8979-202">Az alkalmazások listában válassza ki a **Symantec webes biztonsági szolgáltatás (VSS)**.</span><span class="sxs-lookup"><span data-stu-id="c8979-202">In the applications list, select **Symantec Web Security Service (WSS)**.</span></span>

    ![A Symantec webes biztonsági szolgáltatás (VSS) hivatkozásra az alkalmazások listáját](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_app.png)  

3. <span data-ttu-id="c8979-204">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c8979-204">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="c8979-206">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8979-206">Click **Add** button.</span></span> <span data-ttu-id="c8979-207">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c8979-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="c8979-209">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c8979-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c8979-210">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c8979-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c8979-211">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c8979-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="c8979-212">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c8979-212">Test single sign-on</span></span>

<span data-ttu-id="c8979-213">Ez a szakasz azt tesztelni fogja, az egyszeri bejelentkezés funkció most, hogy konfigurálta az Azure AD SAML-alapú hitelesítéshez használandó WSS fiókját.</span><span class="sxs-lookup"><span data-stu-id="c8979-213">In this section, you'll test the single sign-on functionality now that you've configured your WSS account to use your Azure AD for SAML authentication.</span></span>

<span data-ttu-id="c8979-214">Miután konfigurálta WSS, proxy-forgalom a böngészőben nyissa meg a webböngészőt, és tallózással keresse meg a hely fogja átirányítani az Azure bejelentkezési oldalra, majd próbálja meg.</span><span class="sxs-lookup"><span data-stu-id="c8979-214">After you have configured your web browser to proxy traffic to WSS, when you open your web browser and try to browse to a site then you'll be redirected to the Azure sign-on page.</span></span> <span data-ttu-id="c8979-215">Adja meg az Azure ad (Ez azt jelenti, hogy BrittaSimon) hiba teszt felhasználó hitelesítő adatait, és a kapcsolódó jelszót.</span><span class="sxs-lookup"><span data-stu-id="c8979-215">Enter the credentials of the test end user that has been provisioned in the Azure AD (that is, BrittaSimon) and associated password.</span></span> <span data-ttu-id="c8979-216">Hitelesítését követően lesz a kiválasztott webhelyre navigálhat.</span><span class="sxs-lookup"><span data-stu-id="c8979-216">Once authenticated, you'll be able to browse to the website that you chose.</span></span> <span data-ttu-id="c8979-217">Készítsen házirendszabály a WSS oldalon BrittaSimon letiltása az adott hely számára a böngészési akkor kell megjelennie a WSS-blokk lapon keresse meg, hogy a hely BrittaSimon felhasználóként tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="c8979-217">Should you create a policy rule on the WSS side to block BrittaSimon from browsing to a particular site then you should see the WSS block page when you attempt to browse to that site as user BrittaSimon.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c8979-218">További források</span><span class="sxs-lookup"><span data-stu-id="c8979-218">Additional resources</span></span>

* [<span data-ttu-id="c8979-219">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="c8979-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c8979-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c8979-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_203.png

