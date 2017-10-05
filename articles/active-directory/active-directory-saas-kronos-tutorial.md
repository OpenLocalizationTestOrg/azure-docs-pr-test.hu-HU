---
title: "Oktatóanyag: Azure Active Directoryval integrált Kronos |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Kronos között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e28d6191-c375-43c6-b2df-22daa88d9939
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: eb61ec0a7d3e992a285b1af3d4a7fbe1feb8d991
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kronos"></a><span data-ttu-id="9b19e-103">Oktatóanyag: Azure Active Directoryval integrált Kronos</span><span class="sxs-lookup"><span data-stu-id="9b19e-103">Tutorial: Azure Active Directory integration with Kronos</span></span>

<span data-ttu-id="9b19e-104">Ebben az oktatóanyagban elsajátíthatja Kronos integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9b19e-104">In this tutorial, you learn how to integrate Kronos with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9b19e-105">Kronos integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="9b19e-105">Integrating Kronos with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9b19e-106">Megadhatja a Kronos hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="9b19e-106">You can control in Azure AD who has access to Kronos</span></span>
- <span data-ttu-id="9b19e-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Kronos (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="9b19e-107">You can enable your users to automatically get signed-on to Kronos (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9b19e-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9b19e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9b19e-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9b19e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9b19e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9b19e-110">Prerequisites</span></span>

<span data-ttu-id="9b19e-111">Konfigurálása az Azure AD-integrációs Kronos, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="9b19e-111">To configure Azure AD integration with Kronos, you need the following items:</span></span>

- <span data-ttu-id="9b19e-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9b19e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9b19e-113">A **Kronos munkaerő központi** SSO előfizetés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="9b19e-113">A **Kronos Workforce Central** SSO enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9b19e-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="9b19e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9b19e-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="9b19e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9b19e-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9b19e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9b19e-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9b19e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9b19e-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9b19e-118">Scenario description</span></span>
<span data-ttu-id="9b19e-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9b19e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9b19e-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9b19e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9b19e-121">A gyűjteményből Kronos hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9b19e-121">Adding Kronos from the gallery</span></span>
2. <span data-ttu-id="9b19e-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9b19e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kronos-from-the-gallery"></a><span data-ttu-id="9b19e-123">A gyűjteményből Kronos hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9b19e-123">Adding Kronos from the gallery</span></span>
<span data-ttu-id="9b19e-124">Az Azure AD integrálása a Kronos konfigurálásához kell hozzáadnia Kronos a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="9b19e-124">To configure the integration of Kronos into Azure AD, you need to add Kronos from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9b19e-125">**A gyűjteményből Kronos hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9b19e-125">**To add Kronos from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9b19e-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9b19e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9b19e-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9b19e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9b19e-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9b19e-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="9b19e-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="9b19e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="9b19e-133">Írja be a keresőmezőbe, **Kronos**.</span><span class="sxs-lookup"><span data-stu-id="9b19e-133">In the search box, type **Kronos**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_search.png)

5. <span data-ttu-id="9b19e-135">Az eredmények panelen válassza ki a **Kronos**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9b19e-135">In the results panel, select **Kronos**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9b19e-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9b19e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9b19e-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Kronos</span><span class="sxs-lookup"><span data-stu-id="9b19e-138">In this section, you configure and test Azure AD single sign-on with Kronos based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9b19e-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Kronos a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="9b19e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Kronos is to a user in Azure AD.</span></span> <span data-ttu-id="9b19e-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Kronos közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9b19e-140">In other words, a link relationship between an Azure AD user and the related user in Kronos needs to be established.</span></span>

<span data-ttu-id="9b19e-141">Kronos, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="9b19e-141">In Kronos, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9b19e-142">Az Azure AD egyszeri bejelentkezést a Kronos tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="9b19e-142">To configure and test Azure AD single sign-on with Kronos, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9b19e-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="9b19e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9b19e-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="9b19e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9b19e-145">**[Kronos tesztfelhasználó létrehozása](#creating-a-kronos-test-user)**  - való Britta Simon valami Kronos, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="9b19e-145">**[Creating a Kronos test user](#creating-a-kronos-test-user)** - to have a counterpart of Britta Simon in Kronos that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9b19e-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9b19e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9b19e-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="9b19e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9b19e-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9b19e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9b19e-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Kronos alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9b19e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kronos application.</span></span>

<span data-ttu-id="9b19e-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Kronos, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9b19e-150">**To configure Azure AD single sign-on with Kronos, perform the following steps:**</span></span>

1. <span data-ttu-id="9b19e-151">Az Azure portálon a a **Kronos** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9b19e-151">In the Azure portal, on the **Kronos** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="9b19e-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9b19e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_samlbase.png)

3. <span data-ttu-id="9b19e-155">Az a **Kronos tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="9b19e-155">On the **Kronos Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_url.png)

    <span data-ttu-id="9b19e-157">a.</span><span class="sxs-lookup"><span data-stu-id="9b19e-157">a.</span></span> <span data-ttu-id="9b19e-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.kronos.net/`</span><span class="sxs-lookup"><span data-stu-id="9b19e-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.kronos.net/`</span></span>

    <span data-ttu-id="9b19e-159">b.</span><span class="sxs-lookup"><span data-stu-id="9b19e-159">b.</span></span> <span data-ttu-id="9b19e-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.kronos.net/wfc/navigator/logonWithUID`</span><span class="sxs-lookup"><span data-stu-id="9b19e-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.kronos.net/wfc/navigator/logonWithUID`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9b19e-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="9b19e-161">These values are not real.</span></span> <span data-ttu-id="9b19e-162">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="9b19e-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="9b19e-163">Ügyfél [Kronos támogatási csoport](https://www.kronos.in/contact/en-in/form) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="9b19e-163">Contact [Kronos support team](https://www.kronos.in/contact/en-in/form) to get these values.</span></span>
 
4. <span data-ttu-id="9b19e-164">A Kronos alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="9b19e-164">Your Kronos application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="9b19e-165">Együttműködve [Kronos támogatási csoport](https://www.kronos.in/contact/en-in/form) először a megfelelő felhasználói azonosítót, amely hozzá van rendelve, az alkalmazás azonosításához.</span><span class="sxs-lookup"><span data-stu-id="9b19e-165">Work with [Kronos support team](https://www.kronos.in/contact/en-in/form) first to identify the correct user identifier, which is mapped into the application.</span></span> <span data-ttu-id="9b19e-166">Szánjon is az attribútum, amely szeretnének használni, ez a leképezés vonatkozó útmutatást.</span><span class="sxs-lookup"><span data-stu-id="9b19e-166">Also please take the guidance about the attribute, which they want to use for this mapping.</span></span>
 
     <span data-ttu-id="9b19e-167">A Microsoft azt javasolja, használja a **"NameIdentifier"** attribútum az felhasználói azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="9b19e-167">Microsoft recommends using the **"NameIdentifier"** attribute as user identifier.</span></span> <span data-ttu-id="9b19e-168">Ezek az attribútumok értékének kezelheti a **"Felhasználói attribútumok"** szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="9b19e-168">You can manage the values of these attributes from the **"User Attributes"** section on application integration page.</span></span>
     
     <span data-ttu-id="9b19e-169">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="9b19e-169">The following screenshot shows an example for this.</span></span> <span data-ttu-id="9b19e-170">Itt azt leképezett a **felhasználói azonosító (nameid)** rendelkező **ExtractMailPrefix()** funkciója **user.userprincipalname**.</span><span class="sxs-lookup"><span data-stu-id="9b19e-170">Here we have mapped the **User Identifier (nameid)** with **ExtractMailPrefix()** function of **user.userprincipalname**.</span></span> <span data-ttu-id="9b19e-171">Ez biztosítja, hogy az e-mailek, a felhasználó, amely a egyedi felhasználói azonosító. az előtag értéke</span><span class="sxs-lookup"><span data-stu-id="9b19e-171">This gives the prefix value of email of the user which is the unique User ID.</span></span> <span data-ttu-id="9b19e-172">Ez a Kronos alkalmazását minden sikeres válasz zajlik.</span><span class="sxs-lookup"><span data-stu-id="9b19e-172">This is sent to the Kronos application in every successful response.</span></span> 
     
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_attribute.png)

5. <span data-ttu-id="9b19e-174">Az a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="9b19e-174">In the **User Attributes** section on the **Single sign-on** dialog:</span></span>

    <span data-ttu-id="9b19e-175">a.</span><span class="sxs-lookup"><span data-stu-id="9b19e-175">a.</span></span> <span data-ttu-id="9b19e-176">A felhasználói azonosítóját a legördülő listában válassza ki a **ExtractMailPrefix**.</span><span class="sxs-lookup"><span data-stu-id="9b19e-176">In the User Identifier dropdown list, select **ExtractMailPrefix**.</span></span>

    <span data-ttu-id="9b19e-177">b.</span><span class="sxs-lookup"><span data-stu-id="9b19e-177">b.</span></span> <span data-ttu-id="9b19e-178">Az a **Mail** legördülő listában válassza ki **user.userprincipalname**.</span><span class="sxs-lookup"><span data-stu-id="9b19e-178">In the **Mail** dropdown list, select **user.userprincipalname**.</span></span>

6. <span data-ttu-id="9b19e-179">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9b19e-179">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_certificate.png) 

7. <span data-ttu-id="9b19e-181">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9b19e-181">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kronos-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="9b19e-183">Egyszeri bejelentkezés konfigurálása **Kronos** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [Kronos támogatási csoport](https://www.kronos.in/contact/en-in/form).</span><span class="sxs-lookup"><span data-stu-id="9b19e-183">To configure single sign-on on **Kronos** side, you need to send the downloaded **Metadata XML** to [Kronos support team](https://www.kronos.in/contact/en-in/form).</span></span> 

> [!TIP]
> <span data-ttu-id="9b19e-184">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="9b19e-184">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9b19e-185">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="9b19e-185">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9b19e-186">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9b19e-186">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9b19e-187">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b19e-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="9b19e-188">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="9b19e-188">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="9b19e-190">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9b19e-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9b19e-191">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9b19e-191">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kronos-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9b19e-193">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="9b19e-193">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kronos-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9b19e-195">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="9b19e-195">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kronos-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9b19e-197">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="9b19e-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kronos-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9b19e-199">a.</span><span class="sxs-lookup"><span data-stu-id="9b19e-199">a.</span></span> <span data-ttu-id="9b19e-200">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9b19e-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9b19e-201">b.</span><span class="sxs-lookup"><span data-stu-id="9b19e-201">b.</span></span> <span data-ttu-id="9b19e-202">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9b19e-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9b19e-203">c.</span><span class="sxs-lookup"><span data-stu-id="9b19e-203">c.</span></span> <span data-ttu-id="9b19e-204">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="9b19e-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9b19e-205">d.</span><span class="sxs-lookup"><span data-stu-id="9b19e-205">d.</span></span> <span data-ttu-id="9b19e-206">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9b19e-206">Click **Create**.</span></span>
 
### <a name="creating-a-kronos-test-user"></a><span data-ttu-id="9b19e-207">Kronos tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b19e-207">Creating a Kronos test user</span></span>

<span data-ttu-id="9b19e-208">Ebben a szakaszban egy Kronos Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9b19e-208">In this section, you create a user called Britta Simon in Kronos.</span></span> <span data-ttu-id="9b19e-209">Kronos alkalmazást úgy kell létrehozni, az egyszeri bejelentkezési előtte az alkalmazás minden felhasználó kell.</span><span class="sxs-lookup"><span data-stu-id="9b19e-209">Kronos application needs all the users to be provisioned in the application before doing SSO.</span></span> 

<span data-ttu-id="9b19e-210">Együttműködik az [Kronos támogatási csoport](https://www.kronos.in/contact/en-in/form) ezek a felhasználók az alkalmazás telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="9b19e-210">Work with the [Kronos support team](https://www.kronos.in/contact/en-in/form) to provision all these users into the application.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9b19e-211">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9b19e-211">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9b19e-212">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Kronos Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="9b19e-212">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kronos.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="9b19e-214">**Britta Simon hozzárendelése Kronos, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9b19e-214">**To assign Britta Simon to Kronos, perform the following steps:**</span></span>

1. <span data-ttu-id="9b19e-215">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9b19e-215">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="9b19e-217">Az alkalmazások listában válassza ki a **Kronos**.</span><span class="sxs-lookup"><span data-stu-id="9b19e-217">In the applications list, select **Kronos**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_app.png) 

3. <span data-ttu-id="9b19e-219">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9b19e-219">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="9b19e-221">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9b19e-221">Click **Add** button.</span></span> <span data-ttu-id="9b19e-222">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9b19e-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="9b19e-224">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="9b19e-224">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9b19e-225">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9b19e-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9b19e-226">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9b19e-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9b19e-227">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9b19e-227">Testing single sign-on</span></span>

<span data-ttu-id="9b19e-228">Ebben a szakaszban a Azure AD SSO konfigurációját, a hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="9b19e-228">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="9b19e-229">Ha a hozzáférési panelen Kronos csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Kronos alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="9b19e-229">When you click the Kronos tile in the Access Panel, you should get automatically signed-on to your Kronos application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9b19e-230">További források</span><span class="sxs-lookup"><span data-stu-id="9b19e-230">Additional resources</span></span>

* [<span data-ttu-id="9b19e-231">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="9b19e-231">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9b19e-232">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9b19e-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_203.png

