---
title: "Oktatóanyag: Azure Active Directoryval integrált FileCloud |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és FileCloud között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f39f0ddd-b504-4562-971f-77b88d1e75fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: ad03516f684acc59912ffc57f6e0712828bd03f2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-filecloud"></a><span data-ttu-id="7df17-103">Oktatóanyag: Azure Active Directoryval integrált FileCloud</span><span class="sxs-lookup"><span data-stu-id="7df17-103">Tutorial: Azure Active Directory integration with FileCloud</span></span>

<span data-ttu-id="7df17-104">Ebben az oktatóanyagban elsajátíthatja FileCloud integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7df17-104">In this tutorial, you learn how to integrate FileCloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7df17-105">FileCloud integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="7df17-105">Integrating FileCloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7df17-106">Az Azure AD, aki hozzáfér FileCloud szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="7df17-106">You can control in Azure AD who has access to FileCloud.</span></span>
- <span data-ttu-id="7df17-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett FileCloud (egyszeri bejelentkezés) számára a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="7df17-107">You can enable your users to automatically get signed-on to FileCloud (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="7df17-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="7df17-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="7df17-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7df17-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7df17-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7df17-110">Prerequisites</span></span>

<span data-ttu-id="7df17-111">Konfigurálása az Azure AD-integrációs FileCloud, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="7df17-111">To configure Azure AD integration with FileCloud, you need the following items:</span></span>

- <span data-ttu-id="7df17-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="7df17-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7df17-113">Egy FileCloud egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="7df17-113">A FileCloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7df17-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="7df17-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7df17-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="7df17-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7df17-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7df17-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7df17-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7df17-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7df17-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="7df17-118">Scenario description</span></span>
<span data-ttu-id="7df17-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7df17-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7df17-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="7df17-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7df17-121">A gyűjteményből FileCloud hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7df17-121">Adding FileCloud from the gallery</span></span>
2. <span data-ttu-id="7df17-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7df17-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-filecloud-from-the-gallery"></a><span data-ttu-id="7df17-123">A gyűjteményből FileCloud hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7df17-123">Adding FileCloud from the gallery</span></span>
<span data-ttu-id="7df17-124">Az Azure AD integrálása a FileCloud konfigurálásához kell hozzáadnia FileCloud a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="7df17-124">To configure the integration of FileCloud into Azure AD, you need to add FileCloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7df17-125">**A gyűjteményből FileCloud hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7df17-125">**To add FileCloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7df17-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7df17-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="7df17-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7df17-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7df17-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7df17-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="7df17-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="7df17-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="7df17-133">Írja be a keresőmezőbe, **FileCloud**, jelölje be **FileCloud** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7df17-133">In the search box, type **FileCloud**, select **FileCloud** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában FileCloud](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="7df17-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7df17-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="7df17-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján FileCloud.</span><span class="sxs-lookup"><span data-stu-id="7df17-136">In this section, you configure and test Azure AD single sign-on with FileCloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7df17-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó FileCloud a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="7df17-137">For single sign-on to work, Azure AD needs to know what the counterpart user in FileCloud is to a user in Azure AD.</span></span> <span data-ttu-id="7df17-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a FileCloud közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="7df17-138">In other words, a link relationship between an Azure AD user and the related user in FileCloud needs to be established.</span></span>

<span data-ttu-id="7df17-139">FileCloud, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="7df17-139">In FileCloud, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="7df17-140">Az Azure AD egyszeri bejelentkezést a FileCloud tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="7df17-140">To configure and test Azure AD single sign-on with FileCloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7df17-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="7df17-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7df17-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="7df17-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7df17-143">**[FileCloud tesztfelhasználó létrehozása](#create-a-filecloud-test-user)**  - való Britta Simon valami FileCloud, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="7df17-143">**[Create a FileCloud test user](#create-a-filecloud-test-user)** - to have a counterpart of Britta Simon in FileCloud that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7df17-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="7df17-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7df17-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="7df17-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="7df17-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7df17-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="7df17-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az FileCloud alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7df17-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your FileCloud application.</span></span>

<span data-ttu-id="7df17-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés FileCloud, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7df17-148">**To configure Azure AD single sign-on with FileCloud, perform the following steps:**</span></span>

1. <span data-ttu-id="7df17-149">Az Azure portálon a a **FileCloud** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7df17-149">In the Azure portal, on the **FileCloud** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="7df17-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="7df17-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_samlbase.png)

3. <span data-ttu-id="7df17-153">Az a **FileCloud tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="7df17-153">On the **FileCloud Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információk FileCloud tartomány és az URL-címek](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_url.png)

    <span data-ttu-id="7df17-155">a.</span><span class="sxs-lookup"><span data-stu-id="7df17-155">a.</span></span> <span data-ttu-id="7df17-156">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.filecloudhosted.com`</span><span class="sxs-lookup"><span data-stu-id="7df17-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.filecloudhosted.com`</span></span>

    <span data-ttu-id="7df17-157">b.</span><span class="sxs-lookup"><span data-stu-id="7df17-157">b.</span></span> <span data-ttu-id="7df17-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.filecloudhosted.com/simplesaml/module.php/saml/sp/metadata.php/default-sp`</span><span class="sxs-lookup"><span data-stu-id="7df17-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.filecloudhosted.com/simplesaml/module.php/saml/sp/metadata.php/default-sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7df17-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="7df17-159">These values are not real.</span></span> <span data-ttu-id="7df17-160">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="7df17-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="7df17-161">Ügyfél [FileCloud ügyfél-támogatási csoport](mailto:support@codelathe.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="7df17-161">Contact [FileCloud Client support team](mailto:support@codelathe.com) to get these values.</span></span>

4. <span data-ttu-id="7df17-162">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7df17-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_certificate.png) 

5. <span data-ttu-id="7df17-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7df17-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-filecloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7df17-166">A a **FileCloud konfigurációs** kattintson **konfigurálása FileCloud** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="7df17-166">On the **FileCloud Configuration** section, click **Configure FileCloud** to open **Configure sign-on** window.</span></span> <span data-ttu-id="7df17-167">Másolás a **SAML Entitásazonosító** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="7df17-167">Copy the **SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![FileCloud konfiguráció](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_configure.png) 

7. <span data-ttu-id="7df17-169">Egy másik webes böngészőablakban bejelentkezés a FileCloud bérlő rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7df17-169">In a different web browser window, sign-on to your FileCloud tenant as an administrator.</span></span>

8. <span data-ttu-id="7df17-170">A bal oldali navigációs ablaktábláján kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="7df17-170">On the left navigation pane, click **Settings**.</span></span> 
   
    ![Beállítások szakaszban az alkalmazás ügyféloldali](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_000.png)

9. <span data-ttu-id="7df17-172">Kattintson a **SSO** beállításait szakaszon fülre.</span><span class="sxs-lookup"><span data-stu-id="7df17-172">Click **SSO** tab on Settings section.</span></span> 
   
    ![Egyszeri bejelentkezés lapon az alkalmazás ügyféloldali](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_001.png)

10. <span data-ttu-id="7df17-174">Válassza ki **SAML** , **alapértelmezett egyszeri bejelentkezés típusa** a **egyszeri bejelentkezés (SSO) beállítások** panel.</span><span class="sxs-lookup"><span data-stu-id="7df17-174">Select **SAML** as **Default SSO Type** on **Single Sign On (SSO) Settings** panel.</span></span>
   
    ![Egyszeri bejelentkezés beállítások Panel az alkalmazás ügyféloldali](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_002.png)

11. <span data-ttu-id="7df17-176">Beillesztés **SAML Entitásazonosító**, amely az Azure portálról másolta a **IdP végpont URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="7df17-176">Paste **SAML Entity ID**, which you have copied from Azure portal into the **IdP End Point URL** textbox.</span></span>

    ![IDP End pont URL-cím beviteli mező](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_003.png)

12. <span data-ttu-id="7df17-178">Nyissa meg a letöltött metaadat-fájlt a Jegyzettömbben, annak tartalmának másolása a vágólapra és illessze be azt a **IdP metaadatai** a szövegmező **SAML beállítások** panel.</span><span class="sxs-lookup"><span data-stu-id="7df17-178">Open your downloaded metadata file in notepad, copy the content of it into your clipboard, and then paste it to the **IdP Meta Data** textbox on **SAML Settings** panel.</span></span>

    ![Alkalmazás ügyféloldali IDP Meta adatszakasz](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_004.png)

13. <span data-ttu-id="7df17-180">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7df17-180">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="7df17-181">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="7df17-181">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="7df17-182">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="7df17-182">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="7df17-183">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7df17-183">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="7df17-184">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="7df17-184">Create an Azure AD test user</span></span>

<span data-ttu-id="7df17-185">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="7df17-185">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="7df17-187">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7df17-187">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7df17-188">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="7df17-188">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-filecloud-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="7df17-190">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="7df17-190">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-filecloud-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="7df17-192">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="7df17-192">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-filecloud-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="7df17-194">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="7df17-194">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-filecloud-tutorial/create_aaduser_04.png)

    <span data-ttu-id="7df17-196">a.</span><span class="sxs-lookup"><span data-stu-id="7df17-196">a.</span></span> <span data-ttu-id="7df17-197">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7df17-197">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7df17-198">b.</span><span class="sxs-lookup"><span data-stu-id="7df17-198">b.</span></span> <span data-ttu-id="7df17-199">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7df17-199">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="7df17-200">c.</span><span class="sxs-lookup"><span data-stu-id="7df17-200">c.</span></span> <span data-ttu-id="7df17-201">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="7df17-201">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="7df17-202">d.</span><span class="sxs-lookup"><span data-stu-id="7df17-202">d.</span></span> <span data-ttu-id="7df17-203">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7df17-203">Click **Create**.</span></span>
 
### <a name="create-a-filecloud-test-user"></a><span data-ttu-id="7df17-204">FileCloud tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7df17-204">Create a FileCloud test user</span></span>

<span data-ttu-id="7df17-205">Ez a szakasz célja FileCloud Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="7df17-205">The objective of this section is to create a user called Britta Simon in FileCloud.</span></span> <span data-ttu-id="7df17-206">FileCloud támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="7df17-206">FileCloud supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="7df17-207">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="7df17-207">There is no action item for you in this section.</span></span> <span data-ttu-id="7df17-208">Új felhasználó jön létre az FileCloud elérésére, ha még nem létezik tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="7df17-208">A new user is created during an attempt to access FileCloud if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="7df17-209">Hozza létre a felhasználó manuálisan kell, ha szeretné-e lépjen kapcsolatba a [FileCloud ügyfél-támogatási csoport](mailto:support@codelathe.com).</span><span class="sxs-lookup"><span data-stu-id="7df17-209">If you need to create a user manually, you need to contact the [FileCloud Client support team](mailto:support@codelathe.com).</span></span> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="7df17-210">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="7df17-210">Assign the Azure AD test user</span></span>

<span data-ttu-id="7df17-211">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés FileCloud Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="7df17-211">In this section, you enable Britta Simon to use Azure single sign-on by granting access to FileCloud.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="7df17-213">**Britta Simon hozzárendelése FileCloud, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7df17-213">**To assign Britta Simon to FileCloud, perform the following steps:**</span></span>

1. <span data-ttu-id="7df17-214">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7df17-214">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="7df17-216">Az alkalmazások listában válassza ki a **FileCloud**.</span><span class="sxs-lookup"><span data-stu-id="7df17-216">In the applications list, select **FileCloud**.</span></span>

    ![Az alkalmazások listáját a FileCloud hivatkozás](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_app.png)  

3. <span data-ttu-id="7df17-218">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7df17-218">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="7df17-220">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7df17-220">Click **Add** button.</span></span> <span data-ttu-id="7df17-221">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7df17-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="7df17-223">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="7df17-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7df17-224">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7df17-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7df17-225">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7df17-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="7df17-226">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="7df17-226">Test single sign-on</span></span>

<span data-ttu-id="7df17-227">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="7df17-227">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="7df17-228">Ha a hozzáférési panelen FileCloud csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az FileCloud alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="7df17-228">When you click the FileCloud tile in the Access Panel, you should get automatically signed-on to your FileCloud application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7df17-229">További források</span><span class="sxs-lookup"><span data-stu-id="7df17-229">Additional resources</span></span>

* [<span data-ttu-id="7df17-230">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="7df17-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7df17-231">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="7df17-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_203.png

