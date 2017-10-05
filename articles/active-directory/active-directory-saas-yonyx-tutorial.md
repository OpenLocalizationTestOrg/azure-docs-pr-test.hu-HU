---
title: "Oktatóanyag: Azure Active Directory-integráció Yonyx interaktív útmutatók |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Yonyx interaktív útmutatók között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 07db4e01-319b-4cb6-9b93-4577bffd3cbc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2017
ms.author: jeedes
ms.openlocfilehash: 522f440a0b3746e1101aed845678b3930e030fec
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-yonyx-interactive-guides"></a><span data-ttu-id="f8dae-103">Oktatóanyag: Azure Active Directory-integráció Yonyx interaktív útmutatók</span><span class="sxs-lookup"><span data-stu-id="f8dae-103">Tutorial: Azure Active Directory integration with Yonyx Interactive Guides</span></span>

<span data-ttu-id="f8dae-104">Ebben az oktatóanyagban elsajátíthatja Yonyx interaktív útmutatók integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f8dae-104">In this tutorial, you learn how to integrate Yonyx Interactive Guides with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f8dae-105">Interaktív útmutatók Yonyx integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="f8dae-105">Integrating Yonyx Interactive Guides with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f8dae-106">Szabályozhatja az Azure AD, aki hozzáfér Yonyx interaktív útmutatók</span><span class="sxs-lookup"><span data-stu-id="f8dae-106">You can control in Azure AD who has access to Yonyx Interactive Guides</span></span>
- <span data-ttu-id="f8dae-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Yonyx interaktív útmutatók (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="f8dae-107">You can enable your users to automatically get signed-on to Yonyx Interactive Guides (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f8dae-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f8dae-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="f8dae-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f8dae-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8dae-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f8dae-110">Prerequisites</span></span>

<span data-ttu-id="f8dae-111">Az Azure AD-integrációs konfigurálásához Yonyx interaktív útmutatók, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="f8dae-111">To configure Azure AD integration with Yonyx Interactive Guides, you need the following items:</span></span>

- <span data-ttu-id="f8dae-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f8dae-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f8dae-113">Egy Yonyx interaktív útmutatók az egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="f8dae-113">A Yonyx Interactive Guides single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f8dae-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="f8dae-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f8dae-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="f8dae-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f8dae-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f8dae-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f8dae-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f8dae-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f8dae-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f8dae-118">Scenario description</span></span>
<span data-ttu-id="f8dae-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f8dae-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f8dae-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f8dae-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f8dae-121">A gyűjteményből Yonyx interaktív útmutatók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f8dae-121">Adding Yonyx Interactive Guides from the gallery</span></span>
2. <span data-ttu-id="f8dae-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f8dae-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-yonyx-interactive-guides-from-the-gallery"></a><span data-ttu-id="f8dae-123">A gyűjteményből Yonyx interaktív útmutatók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f8dae-123">Adding Yonyx Interactive Guides from the gallery</span></span>
<span data-ttu-id="f8dae-124">Az Azure AD integrálása a Yonyx interaktív útmutatók konfigurálásához kell hozzáadnia Yonyx interaktív útmutatók a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="f8dae-124">To configure the integration of Yonyx Interactive Guides into Azure AD, you need to add Yonyx Interactive Guides from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f8dae-125">**Adja hozzá a Yonyx interaktív útmutatók a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f8dae-125">**To add Yonyx Interactive Guides from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f8dae-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f8dae-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="f8dae-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f8dae-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f8dae-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f8dae-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="f8dae-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="f8dae-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="f8dae-133">Írja be a keresőmezőbe, **Yonyx interaktív útmutatók**, jelölje be **Yonyx interaktív útmutatók** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f8dae-133">In the search box, type **Yonyx Interactive Guides**, select  **Yonyx Interactive Guides**  from result panel then click **Add** button to add the application.</span></span>

    ![Yonyx interaktív útmutatók az eredménylistában](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="f8dae-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f8dae-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="f8dae-136">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés Yonyx interaktív útmutatók "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="f8dae-136">In this section, you configure and test Azure AD single sign-on with Yonyx Interactive Guides based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f8dae-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Yonyx interaktív útmutatók a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="f8dae-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Yonyx Interactive Guides is to a user in Azure AD.</span></span> <span data-ttu-id="f8dae-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Yonyx interaktív útmutatóban közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f8dae-138">In other words, a link relationship between an Azure AD user and the related user in Yonyx Interactive Guides needs to be established.</span></span>

<span data-ttu-id="f8dae-139">A Yonyx interaktív útmutatóban, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="f8dae-139">In Yonyx Interactive Guides, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="f8dae-140">Az Azure AD az egyszeri bejelentkezés Yonyx interaktív útmutatók tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="f8dae-140">To configure and test Azure AD single sign-on with Yonyx Interactive Guides, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f8dae-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="f8dae-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f8dae-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="f8dae-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f8dae-143">**[Interaktív útmutatók Yonyx tesztfelhasználó létrehozása](#create-a-yonyx-interactive-guides-test-user)**  - kell rendelkeznie a megfelelője a Britta Simon Yonyx interaktív kialakítani, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="f8dae-143">**[Create a Yonyx Interactive Guides test user](#create-a-yonyx-interactive-guides-test-user)** - to have a counterpart of Britta Simon in Yonyx Interactive Guides that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f8dae-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f8dae-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f8dae-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="f8dae-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="f8dae-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f8dae-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="f8dae-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és az interaktív útmutatók Yonyx alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="f8dae-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Yonyx Interactive Guides application.</span></span>

<span data-ttu-id="f8dae-148">**Az Azure AD az egyszeri bejelentkezés Yonyx interaktív útmutatók megadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f8dae-148">**To configure Azure AD single sign-on with Yonyx Interactive Guides, perform the following steps:**</span></span>

1. <span data-ttu-id="f8dae-149">Az Azure portálon a a **Yonyx interaktív útmutatók** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f8dae-149">In the Azure portal, on the **Yonyx Interactive Guides** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="f8dae-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f8dae-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_samlbase.png)

3. <span data-ttu-id="f8dae-153">Az a **Yonyx interaktív útmutatók tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="f8dae-153">On the **Yonyx Interactive Guides Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Yonyx interaktív útmutatók tartomány és az URL-címek](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_url.png)

    <span data-ttu-id="f8dae-155">a.</span><span class="sxs-lookup"><span data-stu-id="f8dae-155">a.</span></span> <span data-ttu-id="f8dae-156">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.yonyx.com/y/conversation/?id=<guid number>`</span><span class="sxs-lookup"><span data-stu-id="f8dae-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.yonyx.com/y/conversation/?id=<guid number>`</span></span>

    <span data-ttu-id="f8dae-157">b.</span><span class="sxs-lookup"><span data-stu-id="f8dae-157">b.</span></span> <span data-ttu-id="f8dae-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.yonyx.com`</span><span class="sxs-lookup"><span data-stu-id="f8dae-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.yonyx.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f8dae-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="f8dae-159">These values are not real.</span></span> <span data-ttu-id="f8dae-160">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="f8dae-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="f8dae-161">Ügyfél [Yonyx interaktív útmutatók ügyfél-támogatási csoport](mailto:support@yonyx.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="f8dae-161">Contact [Yonyx Interactive Guides Client support team](mailto:support@yonyx.com) to get these values.</span></span> 
 
4. <span data-ttu-id="f8dae-162">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f8dae-162">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_certificate.png) 

5. <span data-ttu-id="f8dae-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f8dae-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-yonyx-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f8dae-166">A a **Yonyx interaktív útmutatók konfigurációs** kattintson **Yonyx interaktív útmutatók konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="f8dae-166">On the **Yonyx Interactive Guides Configuration** section, click **Configure Yonyx Interactive Guides** to open **Configure sign-on** window.</span></span> <span data-ttu-id="f8dae-167">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="f8dae-167">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Yonyx interaktív útmutatók konfiguráció](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_configure.png) 

7. <span data-ttu-id="f8dae-169">Egyszeri bejelentkezés konfigurálása **Yonyx interaktív útmutatók** oldalon kell küldeniük a letöltött **Certificate(Base64)**, **Sign-Out URL-cím**, **SAML-alapú egyszeri bejelentkezési URL-címe** **SAML Entitásazonosító** való [Yonyx interaktív útmutatók támogatási csoport](mailto:support@yonyx.com).</span><span class="sxs-lookup"><span data-stu-id="f8dae-169">To configure single sign-on on **Yonyx Interactive Guides** side, you need to send the downloaded **Certificate(Base64)**, **Sign-Out URL**, **SAML Single Sign-On Service URL** **SAML Entity ID** to [Yonyx Interactive Guides support team](mailto:support@yonyx.com).</span></span> <span data-ttu-id="f8dae-170">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="f8dae-170">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="f8dae-171">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="f8dae-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f8dae-172">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="f8dae-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f8dae-173">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f8dae-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="f8dae-174">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="f8dae-174">Create an Azure AD test user</span></span>

<span data-ttu-id="f8dae-175">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="f8dae-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

  ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="f8dae-177">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f8dae-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f8dae-178">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f8dae-178">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-yonyx-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f8dae-180">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f8dae-180">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-yonyx-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f8dae-182">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="f8dae-182">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![A Hozzáadás gombra.](./media/active-directory-saas-yonyx-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f8dae-184">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="f8dae-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![A felhasználó párbeszédpanel](./media/active-directory-saas-yonyx-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f8dae-186">a.</span><span class="sxs-lookup"><span data-stu-id="f8dae-186">a.</span></span> <span data-ttu-id="f8dae-187">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f8dae-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f8dae-188">b.</span><span class="sxs-lookup"><span data-stu-id="f8dae-188">b.</span></span> <span data-ttu-id="f8dae-189">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f8dae-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f8dae-190">c.</span><span class="sxs-lookup"><span data-stu-id="f8dae-190">c.</span></span> <span data-ttu-id="f8dae-191">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="f8dae-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f8dae-192">d.</span><span class="sxs-lookup"><span data-stu-id="f8dae-192">d.</span></span> <span data-ttu-id="f8dae-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f8dae-193">Click **Create**.</span></span>
 
### <a name="create-a-yonyx-interactive-guides-test-user"></a><span data-ttu-id="f8dae-194">Interaktív útmutatók Yonyx tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f8dae-194">Create a Yonyx Interactive Guides test user</span></span>

<span data-ttu-id="f8dae-195">Ez a szakasz célja Yonyx interaktív útmutatók Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f8dae-195">The objective of this section is to create a user called Britta Simon in Yonyx Interactive Guides.</span></span> <span data-ttu-id="f8dae-196">Yonyx interaktív útmutatók támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="f8dae-196">Yonyx Interactive Guides supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="f8dae-197">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="f8dae-197">There is no action item for you in this section.</span></span> <span data-ttu-id="f8dae-198">Új felhasználó jön létre Yonyx interaktív útmutatók elérhetők, ha még nem létezik tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="f8dae-198">A new user is created during an attempt to access Yonyx Interactive Guides if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="f8dae-199">Hozza létre a felhasználó manuálisan kell, ha szeretné-e a Yonyx interaktív útmutatók támogatási csoportjához keresztül < mailto:support@yonyx.com >.</span><span class="sxs-lookup"><span data-stu-id="f8dae-199">If you need to create a user manually, you need to contact the Yonyx Interactive Guides support team via <mailto:support@yonyx.com>.</span></span> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="f8dae-200">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="f8dae-200">Assign the Azure AD test user</span></span>

<span data-ttu-id="f8dae-201">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Yonyx interaktív útmutatók Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="f8dae-201">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Yonyx Interactive Guides.</span></span>

![A felhasználói szerepkör hozzárendelése][200]

<span data-ttu-id="f8dae-203">**Britta Simon hozzárendelése Yonyx interaktív útmutatók, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f8dae-203">**To assign Britta Simon to Yonyx Interactive Guides, perform the following steps:**</span></span>

1. <span data-ttu-id="f8dae-204">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f8dae-204">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f8dae-206">Az alkalmazások listában válassza ki a **Yonyx interaktív útmutatók**.</span><span class="sxs-lookup"><span data-stu-id="f8dae-206">In the applications list, select **Yonyx Interactive Guides**.</span></span>

    ![Az alkalmazások listáját a Yonyx interaktív útmutatók hivatkozás](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_app.png) 

3. <span data-ttu-id="f8dae-208">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f8dae-208">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="f8dae-210">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f8dae-210">Click **Add** button.</span></span> <span data-ttu-id="f8dae-211">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f8dae-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="f8dae-213">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f8dae-213">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f8dae-214">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f8dae-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f8dae-215">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f8dae-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="f8dae-216">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f8dae-216">Test single sign-on</span></span>

<span data-ttu-id="f8dae-217">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="f8dae-217">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f8dae-218">Ha a hozzáférési panelen Yonyx interaktív útmutatók csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Yonyx interaktív útmutatók alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="f8dae-218">When you click the Yonyx Interactive Guides tile in the Access Panel, you should get automatically signed-on to your Yonyx Interactive Guides application.</span></span>

<span data-ttu-id="f8dae-219">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f8dae-219">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f8dae-220">További források</span><span class="sxs-lookup"><span data-stu-id="f8dae-220">Additional resources</span></span>

* [<span data-ttu-id="f8dae-221">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="f8dae-221">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f8dae-222">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f8dae-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_203.png

