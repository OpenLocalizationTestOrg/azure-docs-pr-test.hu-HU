---
title: "Oktatóanyag: Azure Active Directoryval integrált LinkedIn keresési |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a LinkedIn keresési között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a2757a39-1ead-4a3e-91e4-270be3055683
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: e296431866a8611b30e72f286884890adf0f7e50
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-lookup"></a><span data-ttu-id="a9ff7-103">Oktatóanyag: Azure Active Directoryval integrált LinkedIn-keresés</span><span class="sxs-lookup"><span data-stu-id="a9ff7-103">Tutorial: Azure Active Directory integration with LinkedIn Lookup</span></span>

<span data-ttu-id="a9ff7-104">Ebben az oktatóanyagban elsajátíthatja LinkedIn keresési integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a9ff7-104">In this tutorial, you learn how to integrate LinkedIn Lookup with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a9ff7-105">LinkedIn keresési integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="a9ff7-105">Integrating LinkedIn Lookup with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a9ff7-106">Megadhatja a LinkedIn keresési hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="a9ff7-106">You can control in Azure AD who has access to LinkedIn Lookup</span></span>
- <span data-ttu-id="a9ff7-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett LinkedIn megkeresni (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="a9ff7-107">You can enable your users to automatically get signed-on to LinkedIn Lookup (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a9ff7-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a9ff7-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a9ff7-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a9ff7-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a9ff7-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a9ff7-110">Prerequisites</span></span>

<span data-ttu-id="a9ff7-111">Konfigurálása az Azure AD-integrációs LinkedIn-keresés, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="a9ff7-111">To configure Azure AD integration with LinkedIn Lookup, you need the following items:</span></span>

- <span data-ttu-id="a9ff7-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="a9ff7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a9ff7-113">Egy LinkedIn keresési egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="a9ff7-113">An LinkedIn Lookup single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a9ff7-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a9ff7-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="a9ff7-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a9ff7-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a9ff7-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a9ff7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a9ff7-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="a9ff7-118">Scenario description</span></span>
<span data-ttu-id="a9ff7-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a9ff7-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="a9ff7-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a9ff7-121">A gyűjteményből LinkedIn keresési hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a9ff7-121">Adding LinkedIn Lookup from the gallery</span></span>
2. <span data-ttu-id="a9ff7-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a9ff7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-lookup-from-the-gallery"></a><span data-ttu-id="a9ff7-123">A gyűjteményből LinkedIn keresési hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a9ff7-123">Adding LinkedIn Lookup from the gallery</span></span>
<span data-ttu-id="a9ff7-124">Az Azure AD integrálása a LinkedIn keresési konfigurálásához kell hozzáadnia LinkedIn keresési a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-124">To configure the integration of LinkedIn Lookup into Azure AD, you need to add LinkedIn Lookup from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a9ff7-125">**A gyűjteményből LinkedIn keresési hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a9ff7-125">**To add LinkedIn Lookup from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a9ff7-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a9ff7-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a9ff7-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="a9ff7-131">Kattintson a **új alkalmazás** gombra az új alkalmazás hozzáadása párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-131">Click **New application** button on the top of the dialog to add new application.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="a9ff7-133">Írja be a keresőmezőbe, **LinkedIn keresési**.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-133">In the search box, type **LinkedIn Lookup**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_search.png)

5. <span data-ttu-id="a9ff7-135">Az eredmények panelen válassza ki a **LinkedIn keresési**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-135">In the results panel, select **LinkedIn Lookup**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a9ff7-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a9ff7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a9ff7-138">Ebben a szakaszban konfigurálja, és az Azure AD az egyszeri bejelentkezés LinkedIn keresési-teszthez "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-138">In this section, you configure and test Azure AD single sign-on with LinkedIn Lookup based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a9ff7-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a LinkedIn keresési tartozó felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-139">For single sign-on to work, Azure AD needs to know what the counterpart user in LinkedIn Lookup is to a user in Azure AD.</span></span> <span data-ttu-id="a9ff7-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a LinkedIn keresési közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-140">In other words, a link relationship between an Azure AD user and the related user in LinkedIn Lookup needs to be established.</span></span>

<span data-ttu-id="a9ff7-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** LinkedIn-keresés.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in LinkedIn Lookup.</span></span>

<span data-ttu-id="a9ff7-142">Az Azure AD egyszeri bejelentkezést a LinkedIn keresési tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="a9ff7-142">To configure and test Azure AD single sign-on with LinkedIn Lookup, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a9ff7-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a9ff7-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a9ff7-145">**[Egy LinkedIn keresési tesztfelhasználó létrehozása](#creating-an-linkedin-lookup-test-user)**  - való egy megfelelője a Britta Simon LinkedIn keresési, amely kapcsolódik az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-145">**[Creating an LinkedIn Lookup test user](#creating-an-linkedin-lookup-test-user)** - to have a counterpart of Britta Simon in LinkedIn Lookup that is linked to the Azure AD representation.</span></span>
4. <span data-ttu-id="a9ff7-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a9ff7-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a9ff7-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a9ff7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a9ff7-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a LinkedIn keresési alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LinkedIn Lookup application.</span></span>

<span data-ttu-id="a9ff7-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés LinkedIn keresési, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a9ff7-150">**To configure Azure AD single sign-on with LinkedIn Lookup, perform the following steps:**</span></span>

1. <span data-ttu-id="a9ff7-151">Az Azure portálon a a **LinkedIn keresési** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-151">In the Azure portal, on the **LinkedIn Lookup** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="a9ff7-153">Az a **egyszeri bejelentkezés** párbeszédpanelen, a **mód** válasszon **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-153">On the **Single sign-on** dialog, in **Mode** select **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_samlbase.png)

3. <span data-ttu-id="a9ff7-155">Egy másik webes böngészőablakban bejelentkezés a **LinkedIn keresési** webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-155">In a different web browser window, sign-on to your **LinkedIn Lookup** website as an administrator.</span></span>

4. <span data-ttu-id="a9ff7-156">A **Account Center**, kattintson a **globális beállítások** alatt **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-156">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="a9ff7-157">Jelölje ki, **keresési** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-157">Also, select **Lookup** from the dropdown list.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_011.png)

5. <span data-ttu-id="a9ff7-159">Kattintson a **, vagy kattintson ide betölteni, és másolja az űrlap egyes mezőket** , és másolja **entitásazonosító** és **helyességi feltétel felhasználói hozzáférést (ACS) URL-címe**</span><span class="sxs-lookup"><span data-stu-id="a9ff7-159">Click **OR Click Here to load and copy individual fields from the form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_032.png)

6. <span data-ttu-id="a9ff7-161">Az Azure-portál a **LinkedIn keresési tartomány és az URL-címek** területen tegye a következőket, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="a9ff7-161">On Azure portal, under **LinkedIn Lookup Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url.png)

    <span data-ttu-id="a9ff7-163">a.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-163">a.</span></span> <span data-ttu-id="a9ff7-164">A a **azonosító** szövegmező, adja meg a **Entitásazonosító** másolt LinkedIn portálról</span><span class="sxs-lookup"><span data-stu-id="a9ff7-164">In the **Identifier** textbox, enter the **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="a9ff7-165">b.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-165">b.</span></span> <span data-ttu-id="a9ff7-166">A a **válasz URL-CÍMEN** szövegmező, adja meg a **helyességi feltétel felhasználói hozzáférést (ACS) URL-cím** másolt LinkedIn portálról</span><span class="sxs-lookup"><span data-stu-id="a9ff7-166">In the **Reply URL** textbox, enter the **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="a9ff7-167">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="a9ff7-167">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url1.png)

    <span data-ttu-id="a9ff7-169">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://www.linkedIn.com/checkpoint/enterprise/login/<AccountId>?application=lookup`</span><span class="sxs-lookup"><span data-stu-id="a9ff7-169">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://www.linkedIn.com/checkpoint/enterprise/login/<AccountId>?application=lookup`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="a9ff7-170">Ez nem valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-170">This is not real value.</span></span> <span data-ttu-id="a9ff7-171">A felhasználó rendelkezik-e frissíteni ezeket az értékeket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-171">The user has to update these values with the actual Sign-On URL.</span></span> <span data-ttu-id="a9ff7-172">Ügyfél [LinkedIn keresési ügyfél-támogatási csoport](https://business.LinkedIn.com/lookup) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-172">Contact [LinkedIn Lookup Client support team](https://business.LinkedIn.com/lookup) to get this value.</span></span>

8. <span data-ttu-id="a9ff7-173">A **LinkedIn keresési** alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-173">Your **LinkedIn Lookup** application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="a9ff7-174">A felhasználó rendelkezik-e egyéni attribútum-leképezésekhez hozzáadása a SAML-jogkivonat attribútumok konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-174">The user has to add custom attribute mappings to the SAML token attributes configuration.</span></span> <span data-ttu-id="a9ff7-175">Az alábbi képernyőfelvételen szereplő példán látható.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-175">The following screenshot shows an example.</span></span> <span data-ttu-id="a9ff7-176">Az alapértelmezett érték **felhasználói azonosító** van **user.userprincipalname** de LinkedIn keresési vár a képezhető le a felhasználó e-mail címmel.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-176">The default value of **User Identifier** is **user.userprincipalname** but LinkedIn Lookup expects this to be mapped with the user's email address.</span></span> <span data-ttu-id="a9ff7-177">Használhat **user.mail** attribútumot a listából, vagy használja a megfelelő attribútum értéket a szervezet konfiguráció alapján.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-177">You can use **user.mail** attribute from the list or use the appropriate attribute value based on your organization configuration.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/updateusermail.png)
    
9. <span data-ttu-id="a9ff7-179">A **felhasználói attribútumok** kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** és attribútumainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-179">In **User Attributes** section, click **View and edit all other user attributes** and set the attributes.</span></span> <span data-ttu-id="a9ff7-180">A felhasználónak kell nevű négy jogcímeket adhatnak hozzá **e-mail**, **részleg**, **Keresztnév**, és **Vezetéknév** , és le kell képezni a értéke **user.mail**, **felhasználó.részleg**, **user.givenname**, és **user.surname** kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-180">The user needs to add four claims named **email**,  **department**, **firstname**, and **lastname** and the value is to be mapped with **user.mail**, **user.department**, **user.givenname**, and **user.surname** respectively</span></span>

    | <span data-ttu-id="a9ff7-181">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="a9ff7-181">Attribute Name</span></span> | <span data-ttu-id="a9ff7-182">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="a9ff7-182">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="a9ff7-183">E-mailek</span><span class="sxs-lookup"><span data-stu-id="a9ff7-183">email</span></span>| <span data-ttu-id="a9ff7-184">User.mail</span><span class="sxs-lookup"><span data-stu-id="a9ff7-184">user.mail</span></span> |    
    | <span data-ttu-id="a9ff7-185">Szervezeti egység</span><span class="sxs-lookup"><span data-stu-id="a9ff7-185">department</span></span>| <span data-ttu-id="a9ff7-186">felhasználó.részleg</span><span class="sxs-lookup"><span data-stu-id="a9ff7-186">user.department</span></span> |
    | <span data-ttu-id="a9ff7-187">Utónév</span><span class="sxs-lookup"><span data-stu-id="a9ff7-187">firstname</span></span>| <span data-ttu-id="a9ff7-188">User.givenName</span><span class="sxs-lookup"><span data-stu-id="a9ff7-188">user.givenname</span></span> |
    | <span data-ttu-id="a9ff7-189">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="a9ff7-189">lastname</span></span>| <span data-ttu-id="a9ff7-190">User.surname</span><span class="sxs-lookup"><span data-stu-id="a9ff7-190">user.surname</span></span> |

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/userattribute.png)

    <span data-ttu-id="a9ff7-192">a.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-192">a.</span></span> <span data-ttu-id="a9ff7-193">Kattintson a **attribútum hozzáadása** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-193">Click **Add Attribute** to open the **Add Attribute** dialog.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/4.png)
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/5.png)
   
    <span data-ttu-id="a9ff7-196">b.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-196">b.</span></span> <span data-ttu-id="a9ff7-197">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-197">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="a9ff7-198">c.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-198">c.</span></span> <span data-ttu-id="a9ff7-199">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-199">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="a9ff7-200">d.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-200">d.</span></span> <span data-ttu-id="a9ff7-201">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="a9ff7-201">Click **Ok**</span></span>

10. <span data-ttu-id="a9ff7-202">Hajtsa végre a következő lépéseket a **neve** attribútum -</span><span class="sxs-lookup"><span data-stu-id="a9ff7-202">Perform the following steps on the **name** attribute-</span></span>

    <span data-ttu-id="a9ff7-203">a.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-203">a.</span></span> <span data-ttu-id="a9ff7-204">Az attribútum megnyitásához kattintson a **attribútum szerkesztése** ablak.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-204">Click on the attribute to open the **Edit Attribute** window.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/url_update.png)

    <span data-ttu-id="a9ff7-206">b.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-206">b.</span></span> <span data-ttu-id="a9ff7-207">Az URL-cím érték törlése a **névtér**.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-207">Delete the URL value from the **namespace**.</span></span>
    
    <span data-ttu-id="a9ff7-208">c.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-208">c.</span></span> <span data-ttu-id="a9ff7-209">Kattintson a **Ok** a beállítás mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-209">Click **Ok** to save the setting.</span></span>

10. <span data-ttu-id="a9ff7-210">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-210">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_certificate.png) 

11. <span data-ttu-id="a9ff7-212">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-212">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_400.png)

12. <span data-ttu-id="a9ff7-214">Ugrás a **LinkedIn rendszergazdai beállítások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-214">Go to **LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="a9ff7-215">A gombra kattintva Azure-portálról letöltött XML-fájl feltöltése a **feltöltése XML-fájl** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-215">Upload the XML file you downloaded from the Azure portal by clicking the **Upload XML file** option.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_metadata_03.png)

13. <span data-ttu-id="a9ff7-217">Kattintson a **a** SSO engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-217">Click **On** to enable SSO.</span></span> <span data-ttu-id="a9ff7-218">Egyszeri bejelentkezés állapota a **nincs csatlakoztatva** való **csatlakoztatva**</span><span class="sxs-lookup"><span data-stu-id="a9ff7-218">SSO status changes from **Not Connected** to **Connected**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_admin_05.png)

> [!TIP]
> <span data-ttu-id="a9ff7-220">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="a9ff7-220">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a9ff7-221">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-221">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a9ff7-222">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a9ff7-222">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a9ff7-223">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a9ff7-223">Creating an Azure AD test user</span></span>
<span data-ttu-id="a9ff7-224">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-224">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="a9ff7-226">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a9ff7-226">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a9ff7-227">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-227">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a9ff7-229">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-229">Go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a9ff7-231">Kattintson a **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-231">Click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a9ff7-233">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="a9ff7-233">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a9ff7-235">a.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-235">a.</span></span> <span data-ttu-id="a9ff7-236">Az a **neve** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-236">In the **Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="a9ff7-237">b.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-237">b.</span></span> <span data-ttu-id="a9ff7-238">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-238">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="a9ff7-239">c.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-239">c.</span></span> <span data-ttu-id="a9ff7-240">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-240">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a9ff7-241">d.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-241">d.</span></span> <span data-ttu-id="a9ff7-242">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-242">Click **Create**.</span></span>
 
### <a name="creating-an-linkedin-lookup-test-user"></a><span data-ttu-id="a9ff7-243">Egy LinkedIn keresési tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a9ff7-243">Creating an LinkedIn Lookup test user</span></span>

<span data-ttu-id="a9ff7-244">Csatolt keresési alkalmazás támogatja a csak az idő szerinti (JIT) a felhasználók átadása, miután a felhasználók hitelesítésére az alkalmazás automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-244">Linked Lookup Application supports Just in Time (JIT) user provisioning and after authentication users are created in the application automatically.</span></span> <span data-ttu-id="a9ff7-245">Aktiválása **automatikusan a szükséges licencek kiosztása** licenc hozzárendelése a felhasználóhoz.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-245">Activate **Automatically assign licenses** to assign a license to the user.</span></span>
   
   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedin_admin_license.png)

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a9ff7-247">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a9ff7-247">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a9ff7-248">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés LinkedIn keresési Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-248">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LinkedIn Lookup.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="a9ff7-250">**Britta Simon hozzárendelése LinkedIn keresési, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a9ff7-250">**To assign Britta Simon to LinkedIn Lookup, perform the following steps:**</span></span>

1. <span data-ttu-id="a9ff7-251">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-251">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="a9ff7-253">Az alkalmazások listában válassza ki a **LinkedIn keresési**.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-253">In the applications list, select **LinkedIn Lookup**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_app.png) 

3. <span data-ttu-id="a9ff7-255">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-255">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="a9ff7-257">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-257">Click **Add** button.</span></span> <span data-ttu-id="a9ff7-258">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-258">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="a9ff7-260">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-260">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a9ff7-261">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-261">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a9ff7-262">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-262">Click **Assign** button on **Add Assignment** dialog.</span></span>

    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a9ff7-263">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a9ff7-263">Testing single sign-on</span></span>

<span data-ttu-id="a9ff7-264">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-264">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a9ff7-265">Ha a hozzáférési panelen LinkedIn keresési csempére kattint, szervezeti oldalra, ahol meg kell adnia a személyes LinkedIn fiók adatait a rendszer átirányítja.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-265">When you click the LinkedIn Lookup tile in the Access Panel, you should be redirected to Organizational page where you have to provide your personal LinkedIn account details.</span></span> <span data-ttu-id="a9ff7-266">A személyes fiókot a LinkedIn üzleti fiókjával hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="a9ff7-266">It links your personal account with your LinkedIn business account.</span></span> 

<span data-ttu-id="a9ff7-267">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a9ff7-267">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a9ff7-268">További források</span><span class="sxs-lookup"><span data-stu-id="a9ff7-268">Additional resources</span></span>

* [<span data-ttu-id="a9ff7-269">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="a9ff7-269">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a9ff7-270">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="a9ff7-270">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_203.png

