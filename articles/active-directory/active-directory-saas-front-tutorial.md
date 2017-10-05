---
title: "Oktatóanyag: Azure Active Directoryval integrált első |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és az első között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 88270b6d-2571-434a-b139-b6ccc3a2b19f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: d936bc50a66ac2a3c17038ff08351edf9902c99f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-front"></a><span data-ttu-id="6817a-103">Oktatóanyag: Azure Active Directoryval integrált első</span><span class="sxs-lookup"><span data-stu-id="6817a-103">Tutorial: Azure Active Directory integration with Front</span></span>

<span data-ttu-id="6817a-104">Ebben az oktatóanyagban elsajátíthatja első integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6817a-104">In this tutorial, you learn how to integrate Front with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6817a-105">Első integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="6817a-105">Integrating Front with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="6817a-106">Az Azure AD, aki hozzáfér első szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="6817a-106">You can control in Azure AD who has access to Front.</span></span>
- <span data-ttu-id="6817a-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Előrehozás (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="6817a-107">You can enable your users to automatically get signed-on to Front (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="6817a-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="6817a-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="6817a-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6817a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6817a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6817a-110">Prerequisites</span></span>

<span data-ttu-id="6817a-111">Az Azure AD-integráció konfigurálása az első, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="6817a-111">To configure Azure AD integration with Front, you need the following items:</span></span>

- <span data-ttu-id="6817a-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="6817a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6817a-113">Egy első egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="6817a-113">A Front single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6817a-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="6817a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6817a-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="6817a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6817a-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="6817a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6817a-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6817a-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6817a-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="6817a-118">Scenario description</span></span>
<span data-ttu-id="6817a-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="6817a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6817a-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="6817a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6817a-121">Első hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="6817a-121">Adding Front from the gallery</span></span>
2. <span data-ttu-id="6817a-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6817a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-front-from-the-gallery"></a><span data-ttu-id="6817a-123">Első hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="6817a-123">Adding Front from the gallery</span></span>
<span data-ttu-id="6817a-124">Az Azure AD integrálása első konfigurálásához kell hozzáadnia első a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="6817a-124">To configure the integration of Front into Azure AD, you need to add Front from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="6817a-125">**A gyűjtemény első hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6817a-125">**To add Front from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="6817a-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6817a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="6817a-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="6817a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="6817a-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6817a-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="6817a-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="6817a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="6817a-133">Írja be a keresőmezőbe, **első**, jelölje be **első** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6817a-133">In the search box, type **Front**, select **Front** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában első](./media/active-directory-saas-front-tutorial/tutorial_front_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="6817a-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6817a-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="6817a-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés első "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="6817a-136">In this section, you configure and test Azure AD single sign-on with Front based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6817a-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó elöl a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="6817a-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Front is to a user in Azure AD.</span></span> <span data-ttu-id="6817a-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói elöl közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="6817a-138">In other words, a link relationship between an Azure AD user and the related user in Front needs to be established.</span></span>

<span data-ttu-id="6817a-139">Az előtérben, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="6817a-139">In Front, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="6817a-140">Az Azure AD egyszeri bejelentkezést az első tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="6817a-140">To configure and test Azure AD single sign-on with Front, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="6817a-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="6817a-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="6817a-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="6817a-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6817a-143">**[Első tesztfelhasználó létrehozása](#create-a-front-test-user)**  - kell rendelkeznie a megfelelője a Britta Simon elöl, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="6817a-143">**[Create a Front test user](#create-a-front-test-user)** - to have a counterpart of Britta Simon in Front that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="6817a-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6817a-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6817a-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="6817a-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="6817a-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6817a-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="6817a-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és az első alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6817a-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Front application.</span></span>

<span data-ttu-id="6817a-148">**Az Azure AD az egyszeri bejelentkezés konfigurálása az első, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6817a-148">**To configure Azure AD single sign-on with Front, perform the following steps:**</span></span>

1. <span data-ttu-id="6817a-149">Az Azure portálon a a **első** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6817a-149">In the Azure portal, on the **Front** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="6817a-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6817a-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-front-tutorial/tutorial_front_samlbase.png)

3. <span data-ttu-id="6817a-153">Az a **első tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="6817a-153">On the **Front Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_url1.png)

    <span data-ttu-id="6817a-155">a.</span><span class="sxs-lookup"><span data-stu-id="6817a-155">a.</span></span> <span data-ttu-id="6817a-156">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.frontapp.com`</span><span class="sxs-lookup"><span data-stu-id="6817a-156">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.frontapp.com`</span></span>

    <span data-ttu-id="6817a-157">b.</span><span class="sxs-lookup"><span data-stu-id="6817a-157">b.</span></span> <span data-ttu-id="6817a-158">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.frontapp.com/sso/saml/callback`</span><span class="sxs-lookup"><span data-stu-id="6817a-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyname>.frontapp.com/sso/saml/callback`</span></span>

4. <span data-ttu-id="6817a-159">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="6817a-159">Check **Show advanced URL settings**, If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_url2.png)

    <span data-ttu-id="6817a-161">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.frontapp.com`</span><span class="sxs-lookup"><span data-stu-id="6817a-161">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.frontapp.com`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="6817a-162">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="6817a-162">These values are not real.</span></span> <span data-ttu-id="6817a-163">Frissítheti ezeket az értékeket a a tényleges azonosítója, válasz URL-CÍMEN, és a bejelentkezési URL-cím szakaszban találhatók később oktatóanyag, vagy forduljon a [első ügyfél-támogatási csoport](mailto:support@frontapp.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="6817a-163">Update these values with the actual Identifier, Reply URL, and Sign-On URL which are explained later in tutorial or contact [Front Client support team](mailto:support@frontapp.com) to get these values.</span></span> 

5. <span data-ttu-id="6817a-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6817a-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_certificate.png) 

6. <span data-ttu-id="6817a-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="6817a-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="6817a-168">A a **első konfigurációs** kattintson **első konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="6817a-168">On the **Front Configuration** section, click **Configure Front** to open **Configure sign-on** window.</span></span> <span data-ttu-id="6817a-169">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="6817a-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_configure.png) 

8. <span data-ttu-id="6817a-171">Bejelentkezés az első bérlő rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="6817a-171">Sign-on to your Front tenant as an administrator.</span></span>

9. <span data-ttu-id="6817a-172">Ugrás a **beállítások (fogaskerék ikonjára ikonra a bal oldali oldalsávon alján) > Beállítások**.</span><span class="sxs-lookup"><span data-stu-id="6817a-172">Go to **Settings (cog icon at the bottom of the left sidebar) > Preferences**.</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_000.png)

10. <span data-ttu-id="6817a-174">Kattintson a **egyszeri bejelentkezés** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="6817a-174">Click **Single Sign On** link.</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_001.png)

11. <span data-ttu-id="6817a-176">Válassza ki **SAML** legördülő listájában **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6817a-176">Select **SAML** in the drop-down list of **Single Sign On**.</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_002.png)

12. <span data-ttu-id="6817a-178">Az a **belépési pont** szövegmezőbe írja be az értéket a **egyszeri bejelentkezési URL-címe** az Azure AD alkalmazás-konfigurációs varázsló.</span><span class="sxs-lookup"><span data-stu-id="6817a-178">In the **Entry Point** textbox put the value of **Single Sign-on Service URL** from Azure AD application configuration wizard.</span></span>
    
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_003.png)

13. <span data-ttu-id="6817a-180">Nyissa meg a letöltött **Certificate(Base64)** fájlt a Jegyzettömbben, annak tartalmának másolása a vágólapra és illessze be azt a **tanúsítvány aláírása** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="6817a-180">Open your downloaded **Certificate(Base64)** file in notepad, copy the content of it into your clipboard, and then paste it to the **Signing certificate** textbox.</span></span>
    
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_004.png)

14. <span data-ttu-id="6817a-182">Az a **szolgáltató Szolgáltatásbeállítások** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="6817a-182">On the **Service provider settings** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-front-tutorial/tutorial_front_005.png)

    <span data-ttu-id="6817a-184">a.</span><span class="sxs-lookup"><span data-stu-id="6817a-184">a.</span></span> <span data-ttu-id="6817a-185">Értékének másolása **Entitásazonosító** és illessze be azt a **azonosító** textbox **első tartomány és az URL-címek** szakaszban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="6817a-185">Copy the value of **Entity ID** and paste it into the **Identifier** textbox in **Front Domain and URLs** section in Azure portal.</span></span>

    <span data-ttu-id="6817a-186">b.</span><span class="sxs-lookup"><span data-stu-id="6817a-186">b.</span></span> <span data-ttu-id="6817a-187">Értékének másolása **ACS URL-cím** és illessze be azt a **bejelentkezési URL-cím** textbox **első tartomány és az URL-címek** szakaszban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="6817a-187">Copy the value of **ACS URL** and paste it into the **Sign-on URL** textbox in **Front Domain and URLs** section in Azure portal.</span></span>
    
15. <span data-ttu-id="6817a-188">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="6817a-188">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="6817a-189">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="6817a-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="6817a-190">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="6817a-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="6817a-191">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6817a-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="6817a-192">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="6817a-192">Create an Azure AD test user</span></span>

<span data-ttu-id="6817a-193">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="6817a-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="6817a-195">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6817a-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="6817a-196">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="6817a-196">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-front-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="6817a-198">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6817a-198">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-front-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="6817a-200">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="6817a-200">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-front-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="6817a-202">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6817a-202">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-front-tutorial/create_aaduser_04.png)

    <span data-ttu-id="6817a-204">a.</span><span class="sxs-lookup"><span data-stu-id="6817a-204">a.</span></span> <span data-ttu-id="6817a-205">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6817a-205">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6817a-206">b.</span><span class="sxs-lookup"><span data-stu-id="6817a-206">b.</span></span> <span data-ttu-id="6817a-207">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6817a-207">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="6817a-208">c.</span><span class="sxs-lookup"><span data-stu-id="6817a-208">c.</span></span> <span data-ttu-id="6817a-209">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="6817a-209">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="6817a-210">d.</span><span class="sxs-lookup"><span data-stu-id="6817a-210">d.</span></span> <span data-ttu-id="6817a-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6817a-211">Click **Create**.</span></span>
 
### <a name="create-a-front-test-user"></a><span data-ttu-id="6817a-212">Első tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6817a-212">Create a Front test user</span></span>

<span data-ttu-id="6817a-213">Ebben a szakaszban egy Britta Simon elöl nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6817a-213">In this section, you create a user called Britta Simon in Front.</span></span> <span data-ttu-id="6817a-214">Együttműködve [első ügyfél-támogatási csoport](mailto:support@frontapp.com) a felhasználók hozzáadása az első platform.</span><span class="sxs-lookup"><span data-stu-id="6817a-214">Work with [Front Client support team](mailto:support@frontapp.com) to add the users in the Front platform.</span></span> <span data-ttu-id="6817a-215">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="6817a-215">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="6817a-216">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="6817a-216">Assign the Azure AD test user</span></span>

<span data-ttu-id="6817a-217">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés első Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="6817a-217">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Front.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="6817a-219">**Az első Britta Simon rendeléséhez hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6817a-219">**To assign Britta Simon to Front, perform the following steps:**</span></span>

1. <span data-ttu-id="6817a-220">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6817a-220">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="6817a-222">Az alkalmazások listában válassza ki a **első**.</span><span class="sxs-lookup"><span data-stu-id="6817a-222">In the applications list, select **Front**.</span></span>

    ![Az alkalmazások listáját az első hivatkozás](./media/active-directory-saas-front-tutorial/tutorial_front_app.png)  

3. <span data-ttu-id="6817a-224">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="6817a-224">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="6817a-226">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="6817a-226">Click **Add** button.</span></span> <span data-ttu-id="6817a-227">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6817a-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="6817a-229">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="6817a-229">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="6817a-230">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6817a-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6817a-231">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6817a-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="6817a-232">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="6817a-232">Test single sign-on</span></span>

<span data-ttu-id="6817a-233">Ez a szakasz célja tesztelése az Azure AD SSOconfiguration a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="6817a-233">The objective of this section is to test your Azure AD SSOconfiguration using the Access Panel.</span></span>

<span data-ttu-id="6817a-234">Ha az első csempe a hozzáférési panelen gombra kattint, akkor kell beolvasása automatikusan bejelentkezett az első alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="6817a-234">When you click the Front tile in the Access Panel, you should get automatically signed-on to your Front application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="6817a-235">További források</span><span class="sxs-lookup"><span data-stu-id="6817a-235">Additional resources</span></span>

* [<span data-ttu-id="6817a-236">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="6817a-236">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6817a-237">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="6817a-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-front-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-front-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-front-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-front-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-front-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-front-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-front-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-front-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-front-tutorial/tutorial_general_203.png

