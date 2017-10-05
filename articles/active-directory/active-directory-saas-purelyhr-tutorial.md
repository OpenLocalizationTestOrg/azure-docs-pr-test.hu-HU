---
title: "Oktatóanyag: Azure Active Directoryval integrált PurelyHR |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és PurelyHR között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 86a9c454-596d-4902-829a-fe126708f739
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: a9075b1759ebd39f164bfe288fb0a365acdcc44c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-purelyhr"></a><span data-ttu-id="821c3-103">Oktatóanyag: Azure Active Directoryval integrált PurelyHR</span><span class="sxs-lookup"><span data-stu-id="821c3-103">Tutorial: Azure Active Directory integration with PurelyHR</span></span>

<span data-ttu-id="821c3-104">Ebben az oktatóanyagban elsajátíthatja PurelyHR integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="821c3-104">In this tutorial, you learn how to integrate PurelyHR with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="821c3-105">PurelyHR integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="821c3-105">Integrating PurelyHR with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="821c3-106">Megadhatja a PurelyHR hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="821c3-106">You can control in Azure AD who has access to PurelyHR</span></span>
- <span data-ttu-id="821c3-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett PurelyHR (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="821c3-107">You can enable your users to automatically get signed-on to PurelyHR (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="821c3-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="821c3-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="821c3-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="821c3-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="821c3-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="821c3-110">Prerequisites</span></span>

<span data-ttu-id="821c3-111">Konfigurálása az Azure AD-integrációs PurelyHR, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="821c3-111">To configure Azure AD integration with PurelyHR, you need the following items:</span></span>

- <span data-ttu-id="821c3-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="821c3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="821c3-113">Egy PurelyHR egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="821c3-113">A PurelyHR single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="821c3-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="821c3-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="821c3-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="821c3-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="821c3-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="821c3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="821c3-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="821c3-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="821c3-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="821c3-118">Scenario description</span></span>
<span data-ttu-id="821c3-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="821c3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="821c3-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="821c3-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="821c3-121">A gyűjteményből PurelyHR hozzáadása</span><span class="sxs-lookup"><span data-stu-id="821c3-121">Adding PurelyHR from the gallery</span></span>
2. <span data-ttu-id="821c3-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="821c3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-purelyhr-from-the-gallery"></a><span data-ttu-id="821c3-123">A gyűjteményből PurelyHR hozzáadása</span><span class="sxs-lookup"><span data-stu-id="821c3-123">Adding PurelyHR from the gallery</span></span>
<span data-ttu-id="821c3-124">Az Azure AD integrálása a PurelyHR konfigurálásához kell hozzáadnia PurelyHR a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="821c3-124">To configure the integration of PurelyHR into Azure AD, you need to add PurelyHR from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="821c3-125">**A gyűjteményből PurelyHR hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="821c3-125">**To add PurelyHR from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="821c3-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="821c3-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="821c3-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="821c3-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="821c3-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="821c3-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="821c3-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="821c3-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="821c3-133">Írja be a keresőmezőbe, **PurelyHR**.</span><span class="sxs-lookup"><span data-stu-id="821c3-133">In the search box, type **PurelyHR**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_search.png)

5. <span data-ttu-id="821c3-135">Az eredmények panelen válassza ki a **PurelyHR**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="821c3-135">In the results panel, select **PurelyHR**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="821c3-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="821c3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="821c3-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján PurelyHR</span><span class="sxs-lookup"><span data-stu-id="821c3-138">In this section, you configure and test Azure AD single sign-on with PurelyHR based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="821c3-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó PurelyHR a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="821c3-139">For single sign-on to work, Azure AD needs to know what the counterpart user in PurelyHR is to a user in Azure AD.</span></span> <span data-ttu-id="821c3-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a PurelyHR közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="821c3-140">In other words, a link relationship between an Azure AD user and the related user in PurelyHR needs to be established.</span></span>

<span data-ttu-id="821c3-141">PurelyHR, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="821c3-141">In PurelyHR, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="821c3-142">Az Azure AD egyszeri bejelentkezést a PurelyHR tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="821c3-142">To configure and test Azure AD single sign-on with PurelyHR, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="821c3-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="821c3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="821c3-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="821c3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="821c3-145">**[PurelyHR tesztfelhasználó létrehozása](#creating-a-purelyhr-test-user)**  - való Britta Simon valami PurelyHR, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="821c3-145">**[Creating a PurelyHR test user](#creating-a-purelyhr-test-user)** - to have a counterpart of Britta Simon in PurelyHR that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="821c3-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="821c3-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="821c3-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="821c3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="821c3-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="821c3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="821c3-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az PurelyHR alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="821c3-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your PurelyHR application.</span></span>

<span data-ttu-id="821c3-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés PurelyHR, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="821c3-150">**To configure Azure AD single sign-on with PurelyHR, perform the following steps:**</span></span>

1. <span data-ttu-id="821c3-151">Az Azure portálon a a **PurelyHR** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="821c3-151">In the Azure portal, on the **PurelyHR** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="821c3-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="821c3-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_samlbase.png)

3. <span data-ttu-id="821c3-155">Az a **PurelyHR tartomány és az URL-címek** területen tegye a következőket, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="821c3-155">On the **PurelyHR Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_url.png)
   
    <span data-ttu-id="821c3-157">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyID>.purelyhr.com/sso-consume`</span><span class="sxs-lookup"><span data-stu-id="821c3-157">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyID>.purelyhr.com/sso-consume`</span></span>

4. <span data-ttu-id="821c3-158">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="821c3-158">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_url1.png)
    
    <span data-ttu-id="821c3-160">Az a **bejelentkezési URL-cím** szövegmező, írja be az értéket a következő minta használatával:`https://<companyID>.purelyhr.com/sso-initiate`</span><span class="sxs-lookup"><span data-stu-id="821c3-160">In the **Sign-on URL** textbox, type the value using the following pattern: `https://<companyID>.purelyhr.com/sso-initiate`</span></span>
     
    > [!NOTE]
    > <span data-ttu-id="821c3-161">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="821c3-161">These values are not the real.</span></span> <span data-ttu-id="821c3-162">Frissítheti ezeket az értékeket a tényleges válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="821c3-162">Update these values with the actual Reply URL and Sign-On URL.</span></span> <span data-ttu-id="821c3-163">Ügyfél [PurelyHR ügyfél-támogatási csoport](http://support.purelyhr.com/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="821c3-163">Contact [PurelyHR Client support team](http://support.purelyhr.com/) to get these values.</span></span> 

5. <span data-ttu-id="821c3-164">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="821c3-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_certificate.png) 

6. <span data-ttu-id="821c3-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="821c3-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-purelyhr-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="821c3-168">A a **PurelyHR konfigurációs** kattintson **konfigurálása PurelyHR** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="821c3-168">On the **PurelyHR Configuration** section, click **Configure PurelyHR** to open **Configure sign-on** window.</span></span> <span data-ttu-id="821c3-169">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="821c3-169">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_configure.png) 

8. <span data-ttu-id="821c3-171">Egyszeri bejelentkezés konfigurálása **PurelyHR** ügyféloldali, jelentkezzen be rendszergazdaként a webhelyen.</span><span class="sxs-lookup"><span data-stu-id="821c3-171">To configure single sign-on on **PurelyHR** side, login to their website as an administrator.</span></span>

9. <span data-ttu-id="821c3-172">Nyissa meg a **irányítópult** a eszköztárán, majd kattintson a beállítások **egyszeri bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="821c3-172">Open the **Dashboard** from the options in the toolbar and click **SSO Settings**.</span></span>

10. <span data-ttu-id="821c3-173">Illessze be az értékeket a mezőkbe, lásd az alábbi-</span><span class="sxs-lookup"><span data-stu-id="821c3-173">Paste the values in the boxes as described below-</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-purelyhr-tutorial/purelyhr-dashboard-sso-settings.png)    

    <span data-ttu-id="821c3-175">a.</span><span class="sxs-lookup"><span data-stu-id="821c3-175">a.</span></span> <span data-ttu-id="821c3-176">Nyissa meg a **Certificate(Bas64)** a Jegyzettömbben az Azure portálról letöltött, és másolja a tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="821c3-176">Open the **Certificate(Bas64)** downloaded from the Azure portal in notepad and copy the certificate value.</span></span> <span data-ttu-id="821c3-177">Illessze be a másolt érték a **X.509 tanúsítvány** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="821c3-177">Paste the copied value into the **X.509 Certificate** box.</span></span>

    <span data-ttu-id="821c3-178">b.</span><span class="sxs-lookup"><span data-stu-id="821c3-178">b.</span></span> <span data-ttu-id="821c3-179">Az a **Idp kiállítójának URL-címe** mezőbe illessze be a **SAML Entitásazonosító** másolta át az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="821c3-179">In the **Idp Issuer URL** box, paste the **SAML Entity ID** copied from the Azure portal.</span></span>

    <span data-ttu-id="821c3-180">c.</span><span class="sxs-lookup"><span data-stu-id="821c3-180">c.</span></span> <span data-ttu-id="821c3-181">Az a **Idp végponti URL-cím** mezőbe illessze be a **SAML-alapú egyszeri bejelentkezési URL-címe** másolta át az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="821c3-181">In the **Idp Endpoint URL** box, paste the **SAML Single Sign-On Service URL** copied from the Azure portal.</span></span> 

    <span data-ttu-id="821c3-182">d.</span><span class="sxs-lookup"><span data-stu-id="821c3-182">d.</span></span> <span data-ttu-id="821c3-183">Ellenőrizze a **automatikus létrehozása felhasználók** automatikus felhasználói PurelyHR a kiépítés engedélyezése jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="821c3-183">Check the **Auto-Create Users** checkbox to enable automatic user provisioning in PurelyHR.</span></span>

    <span data-ttu-id="821c3-184">e.</span><span class="sxs-lookup"><span data-stu-id="821c3-184">e.</span></span> <span data-ttu-id="821c3-185">Kattintson a **módosítások mentése** menti a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="821c3-185">Click **Save Changes** to save the settings.</span></span>

> [!TIP]
> <span data-ttu-id="821c3-186">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="821c3-186">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="821c3-187">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="821c3-187">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="821c3-188">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="821c3-188">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="821c3-189">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="821c3-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="821c3-190">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="821c3-190">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="821c3-192">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="821c3-192">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="821c3-193">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="821c3-193">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="821c3-195">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="821c3-195">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="821c3-197">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="821c3-197">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="821c3-199">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="821c3-199">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="821c3-201">a.</span><span class="sxs-lookup"><span data-stu-id="821c3-201">a.</span></span> <span data-ttu-id="821c3-202">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="821c3-202">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="821c3-203">b.</span><span class="sxs-lookup"><span data-stu-id="821c3-203">b.</span></span> <span data-ttu-id="821c3-204">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="821c3-204">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="821c3-205">c.</span><span class="sxs-lookup"><span data-stu-id="821c3-205">c.</span></span> <span data-ttu-id="821c3-206">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="821c3-206">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="821c3-207">d.</span><span class="sxs-lookup"><span data-stu-id="821c3-207">d.</span></span> <span data-ttu-id="821c3-208">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="821c3-208">Click **Create**.</span></span>
 
### <a name="creating-a-purelyhr-test-user"></a><span data-ttu-id="821c3-209">PurelyHR tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="821c3-209">Creating a PurelyHR test user</span></span>

<span data-ttu-id="821c3-210">Ahhoz, hogy az Azure AD-felhasználók PurelyHR bejelentkezni, akkor ki kell építenie a PurelyHR.</span><span class="sxs-lookup"><span data-stu-id="821c3-210">To enable Azure AD users to log in to PurelyHR, they must be provisioned into PurelyHR.</span></span> <span data-ttu-id="821c3-211">Egy automatikus feladat PurelyHR, és nincs manuális lépések szükségesek, ha engedélyezve van az automatikus felhasználólétesítés.</span><span class="sxs-lookup"><span data-stu-id="821c3-211">In PurelyHR, provisioning is an automatic task and no manual steps are required when automatic user provisioning is enabled.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="821c3-212">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="821c3-212">Assigning the Azure AD test user</span></span>

<span data-ttu-id="821c3-213">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés PurelyHR Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="821c3-213">In this section, you enable Britta Simon to use Azure single sign-on by granting access to PurelyHR.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="821c3-215">**Britta Simon hozzárendelése PurelyHR, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="821c3-215">**To assign Britta Simon to PurelyHR, perform the following steps:**</span></span>

1. <span data-ttu-id="821c3-216">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="821c3-216">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="821c3-218">Az alkalmazások listában válassza ki a **PurelyHR**.</span><span class="sxs-lookup"><span data-stu-id="821c3-218">In the applications list, select **PurelyHR**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_app.png) 

3. <span data-ttu-id="821c3-220">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="821c3-220">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="821c3-222">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="821c3-222">Click **Add** button.</span></span> <span data-ttu-id="821c3-223">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="821c3-223">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="821c3-225">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="821c3-225">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="821c3-226">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="821c3-226">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="821c3-227">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="821c3-227">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="821c3-228">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="821c3-228">Testing single sign-on</span></span>

<span data-ttu-id="821c3-229">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="821c3-229">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="821c3-230">A felvegye LMS csempe a hozzáférési panelen kattintson, akkor beolvasása automatikusan bejelentkezett az felvegye LMS alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="821c3-230">Click the Absorb LMS tile in the Access Panel, you get automatically signed-on to your Absorb LMS application.</span></span>

<span data-ttu-id="821c3-231">A hozzáférési Panel kapcsolatos további információkért tekintse meg.</span><span class="sxs-lookup"><span data-stu-id="821c3-231">For more information about the Access Panel, see.</span></span> <span data-ttu-id="821c3-232">[A hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="821c3-232">[Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="821c3-233">További források</span><span class="sxs-lookup"><span data-stu-id="821c3-233">Additional resources</span></span>

* [<span data-ttu-id="821c3-234">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="821c3-234">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="821c3-235">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="821c3-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_203.png

