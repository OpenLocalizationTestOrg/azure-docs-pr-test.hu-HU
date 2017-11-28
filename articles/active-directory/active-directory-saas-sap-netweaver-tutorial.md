---
title: "Oktatóanyag: Azure Active Directoryval integrált SAP NetWeaver |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és az SAP NetWeaver között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1b9e59e3-e7ae-4e74-b16c-8c1a7ccfdef3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: ad4140eb1183094a67822ad92eabcd35101360b6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-netweaver"></a><span data-ttu-id="47edc-103">Oktatóanyag: Azure Active Directory-integráció az SAP NetWeaver megoldással</span><span class="sxs-lookup"><span data-stu-id="47edc-103">Tutorial: Azure Active Directory integration with SAP NetWeaver</span></span>

<span data-ttu-id="47edc-104">Ebben az oktatóanyagban elsajátíthatja SAP NetWeaver integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="47edc-104">In this tutorial, you learn how to integrate SAP NetWeaver with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="47edc-105">SAP NetWeaver integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="47edc-105">Integrating SAP NetWeaver with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="47edc-106">Megadhatja a SAP NetWeaver hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="47edc-106">You can control in Azure AD who has access to SAP NetWeaver</span></span>
- <span data-ttu-id="47edc-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett az SAP NetWeaver (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="47edc-107">You can enable your users to automatically get signed-on to SAP NetWeaver (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="47edc-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="47edc-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="47edc-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="47edc-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="47edc-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="47edc-110">Prerequisites</span></span>

<span data-ttu-id="47edc-111">SAP NetWeaver konfigurálása az Azure AD-integrációs, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="47edc-111">To configure Azure AD integration with SAP NetWeaver, you need the following items:</span></span>

- <span data-ttu-id="47edc-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="47edc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="47edc-113">Egy SAP NetWeaver egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="47edc-113">An SAP NetWeaver single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="47edc-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="47edc-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="47edc-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="47edc-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="47edc-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="47edc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="47edc-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="47edc-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="47edc-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="47edc-118">Scenario description</span></span>
<span data-ttu-id="47edc-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="47edc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="47edc-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="47edc-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="47edc-121">SAP NetWeaver hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="47edc-121">Adding SAP NetWeaver from the gallery</span></span>
2. <span data-ttu-id="47edc-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="47edc-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sap-netweaver-from-the-gallery"></a><span data-ttu-id="47edc-123">SAP NetWeaver hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="47edc-123">Adding SAP NetWeaver from the gallery</span></span>
<span data-ttu-id="47edc-124">Az Azure AD integrálása a SAP NetWeaver konfigurálásához kell hozzáadnia SAP NetWeaver a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="47edc-124">To configure the integration of SAP NetWeaver into Azure AD, you need to add SAP NetWeaver from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="47edc-125">**SAP NetWeaver hozzáadása a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="47edc-125">**To add SAP NetWeaver from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="47edc-126">Az a  **[Azure Portal](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="47edc-126">In the **[Azure Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="47edc-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="47edc-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="47edc-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="47edc-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="47edc-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="47edc-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="47edc-133">Írja be a keresőmezőbe, **SAP NetWeaver**.</span><span class="sxs-lookup"><span data-stu-id="47edc-133">In the search box, type **SAP NetWeaver**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_search.png)

5. <span data-ttu-id="47edc-135">Az eredmények panelen válassza ki a **SAP NetWeaver**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="47edc-135">In the results panel, select **SAP NetWeaver**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="47edc-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="47edc-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="47edc-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést az SAP NetWeaver "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="47edc-138">In this section, you configure and test Azure AD single sign-on with SAP NetWeaver based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="47edc-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a SAP NetWeaver tartozó felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="47edc-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SAP NetWeaver is to a user in Azure AD.</span></span> <span data-ttu-id="47edc-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a SAP NetWeaver közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="47edc-140">In other words, a link relationship between an Azure AD user and the related user in SAP NetWeaver needs to be established.</span></span>

<span data-ttu-id="47edc-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** az SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="47edc-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in SAP NetWeaver.</span></span>

<span data-ttu-id="47edc-142">Az Azure AD egyszeri bejelentkezést az SAP NetWeaver tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="47edc-142">To configure and test Azure AD single sign-on with SAP NetWeaver, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="47edc-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="47edc-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="47edc-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="47edc-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="47edc-145">**[Egy SAP NetWeaver tesztfelhasználó létrehozása](#creating-an-sap-netweaver-test-user)**  - való egy megfelelője a Britta Simon SAP NetWeaver, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="47edc-145">**[Creating an SAP NetWeaver test user](#creating-an-sap-netweaver-test-user)** - to have a counterpart of Britta Simon in SAP NetWeaver that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="47edc-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="47edc-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="47edc-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="47edc-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="47edc-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="47edc-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="47edc-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és az SAP NetWeaver alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="47edc-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SAP NetWeaver application.</span></span>

<span data-ttu-id="47edc-150">**SAP NetWeaver konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="47edc-150">**To configure Azure AD single sign-on with SAP NetWeaver, perform the following steps:**</span></span>

1. <span data-ttu-id="47edc-151">Az Azure portálon a a **SAP NetWeaver** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="47edc-151">In the Azure portal, on the **SAP NetWeaver** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="47edc-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="47edc-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_samlbase.png)

3. <span data-ttu-id="47edc-155">Az a **SAP NetWeaver tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="47edc-155">On the **SAP NetWeaver Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_url.png)

    <span data-ttu-id="47edc-157">a.</span><span class="sxs-lookup"><span data-stu-id="47edc-157">a.</span></span> <span data-ttu-id="47edc-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<your company instance of SAP NetWeaver>`</span><span class="sxs-lookup"><span data-stu-id="47edc-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<your company instance of SAP NetWeaver>`</span></span>

    <span data-ttu-id="47edc-159">b.</span><span class="sxs-lookup"><span data-stu-id="47edc-159">b.</span></span> <span data-ttu-id="47edc-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<your company instance of SAP NetWeaver>`</span><span class="sxs-lookup"><span data-stu-id="47edc-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<your company instance of SAP NetWeaver>`</span></span>

    <span data-ttu-id="47edc-161">c.</span><span class="sxs-lookup"><span data-stu-id="47edc-161">c.</span></span> <span data-ttu-id="47edc-162">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<your company instance of SAP NetWeaver>/sap/saml2/sp/acs/100`</span><span class="sxs-lookup"><span data-stu-id="47edc-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://<your company instance of SAP NetWeaver>/sap/saml2/sp/acs/100`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="47edc-163">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="47edc-163">These values are not the real.</span></span> <span data-ttu-id="47edc-164">Frissítheti ezeket az értékeket a tényleges azonosítója és válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="47edc-164">Update these values with the actual Identifier and Reply URL and Sign-On URL.</span></span> <span data-ttu-id="47edc-165">Itt javasoljuk, hogy az azonosító a karakterlánc egyedi értéket használja.</span><span class="sxs-lookup"><span data-stu-id="47edc-165">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="47edc-166">Ügyfél [SAP NetWeaver ügyfél-támogatási csoport](https://www.sap.com/support.html) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="47edc-166">Contact [SAP NetWeaver Client support team](https://www.sap.com/support.html) to get these values.</span></span> 

4. <span data-ttu-id="47edc-167">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="47edc-167">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_certificate.png) 

5. <span data-ttu-id="47edc-169">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="47edc-169">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="47edc-171">A a **SAP NetWeaver konfigurációs** kattintson **konfigurálása SAP NetWeaver** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="47edc-171">On the **SAP NetWeaver Configuration** section, click **Configure SAP NetWeaver** to open **Configure sign-on** window.</span></span> <span data-ttu-id="47edc-172">Másolás a **SAML Entitásazonosító** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="47edc-172">Copy the **SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_configure.png) 

7. <span data-ttu-id="47edc-174">Egyszeri bejelentkezés konfigurálása **SAP NetWeaver** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** és **SAML Entitásazonosító** való [SAP NetWeaver támogatási](https://www.sap.com/support.html).</span><span class="sxs-lookup"><span data-stu-id="47edc-174">To configure single sign-on on **SAP NetWeaver** side, you need to send the downloaded **Metadata XML** and **SAML Entity ID** to [SAP NetWeaver support](https://www.sap.com/support.html).</span></span> 

> [!TIP]
> <span data-ttu-id="47edc-175">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="47edc-175">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="47edc-176">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="47edc-176">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="47edc-177">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="47edc-177">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="47edc-178">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="47edc-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="47edc-179">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="47edc-179">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="47edc-181">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="47edc-181">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="47edc-182">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="47edc-182">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="47edc-184">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="47edc-184">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="47edc-186">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="47edc-186">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="47edc-188">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="47edc-188">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="47edc-190">a.</span><span class="sxs-lookup"><span data-stu-id="47edc-190">a.</span></span> <span data-ttu-id="47edc-191">Az a **neve** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="47edc-191">In the **Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="47edc-192">b.</span><span class="sxs-lookup"><span data-stu-id="47edc-192">b.</span></span> <span data-ttu-id="47edc-193">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="47edc-193">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="47edc-194">c.</span><span class="sxs-lookup"><span data-stu-id="47edc-194">c.</span></span> <span data-ttu-id="47edc-195">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="47edc-195">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="47edc-196">d.</span><span class="sxs-lookup"><span data-stu-id="47edc-196">d.</span></span> <span data-ttu-id="47edc-197">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="47edc-197">Click **Create**.</span></span>
 
### <a name="creating-an-sap-netweaver-test-user"></a><span data-ttu-id="47edc-198">Egy SAP NetWeaver tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="47edc-198">Creating an SAP NetWeaver test user</span></span>

<span data-ttu-id="47edc-199">Ebben a szakaszban egy SAP NetWeaver Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="47edc-199">In this section, you create a user called Britta Simon in SAP NetWeaver.</span></span> <span data-ttu-id="47edc-200">Együttműködik a [SAP NetWeaver támogatási](https://www.sap.com/support.html) a felhasználók hozzáadása az SAP NetWeaver platform.</span><span class="sxs-lookup"><span data-stu-id="47edc-200">Work with your [SAP NetWeaver support](https://www.sap.com/support.html) to add the users in the SAP NetWeaver platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="47edc-201">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="47edc-201">Assigning the Azure AD test user</span></span>

<span data-ttu-id="47edc-202">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés SAP NetWeaver Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="47edc-202">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SAP NetWeaver.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="47edc-204">**SAP NetWeaver Britta Simon rendel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="47edc-204">**To assign Britta Simon to SAP NetWeaver, perform the following steps:**</span></span>

1. <span data-ttu-id="47edc-205">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="47edc-205">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="47edc-207">Az alkalmazások listában válassza ki a **SAP NetWeaver**.</span><span class="sxs-lookup"><span data-stu-id="47edc-207">In the applications list, select **SAP NetWeaver**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_app.png) 

3. <span data-ttu-id="47edc-209">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="47edc-209">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="47edc-211">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="47edc-211">Click **Add** button.</span></span> <span data-ttu-id="47edc-212">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="47edc-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="47edc-214">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="47edc-214">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="47edc-215">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="47edc-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="47edc-216">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="47edc-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="47edc-217">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="47edc-217">Testing single sign-on</span></span>

<span data-ttu-id="47edc-218">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="47edc-218">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="47edc-219">Ha a hozzáférési panelen SAP NetWeaver csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az SAP NetWeaver alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="47edc-219">When you click the SAP NetWeaver tile in the Access Panel, you should get automatically signed-on to your SAP NetWeaver application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="47edc-220">További források</span><span class="sxs-lookup"><span data-stu-id="47edc-220">Additional resources</span></span>

* [<span data-ttu-id="47edc-221">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="47edc-221">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="47edc-222">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="47edc-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_203.png

