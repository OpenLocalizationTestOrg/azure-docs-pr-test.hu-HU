---
title: "Oktatóanyag: Azure Active Directoryval integrált felvegye LMS |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és felvegye LMS között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 3c68c3ac7d6be593476d419f8c015931b206eead
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a><span data-ttu-id="b8aec-103">Oktatóanyag: Azure Active Directoryval integrált felvegye LMS</span><span class="sxs-lookup"><span data-stu-id="b8aec-103">Tutorial: Azure Active Directory integration with Absorb LMS</span></span>

<span data-ttu-id="b8aec-104">Ebben az oktatóanyagban elsajátíthatja felvegye LMS integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b8aec-104">In this tutorial, you learn how to integrate Absorb LMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b8aec-105">Felvegye LMS integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="b8aec-105">Integrating Absorb LMS with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b8aec-106">Szabályozhatja, hogy felvegye LMS hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="b8aec-106">You can control in Azure AD who has access to Absorb LMS</span></span>
- <span data-ttu-id="b8aec-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a felvegye LMS (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="b8aec-107">You can enable your users to automatically get signed-on to Absorb LMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b8aec-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b8aec-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b8aec-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg.</span><span class="sxs-lookup"><span data-stu-id="b8aec-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="b8aec-110">[Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b8aec-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b8aec-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b8aec-111">Prerequisites</span></span>

<span data-ttu-id="b8aec-112">Az Azure AD-integrációs felvegye LMS konfigurálni, kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="b8aec-112">To configure Azure AD integration with Absorb LMS, you need the following items:</span></span>

- <span data-ttu-id="b8aec-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b8aec-113">An Azure AD subscription</span></span>
- <span data-ttu-id="b8aec-114">Egy felvegye LMS egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="b8aec-114">An Absorb LMS single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b8aec-115">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="b8aec-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b8aec-116">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="b8aec-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b8aec-117">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b8aec-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b8aec-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b8aec-118">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b8aec-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b8aec-119">Scenario description</span></span>
<span data-ttu-id="b8aec-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b8aec-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b8aec-121">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b8aec-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b8aec-122">A gyűjteményből felvegye LMS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b8aec-122">Adding Absorb LMS from the gallery</span></span>
2. <span data-ttu-id="b8aec-123">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b8aec-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-absorb-lms-from-the-gallery"></a><span data-ttu-id="b8aec-124">A gyűjteményből felvegye LMS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b8aec-124">Adding Absorb LMS from the gallery</span></span>
<span data-ttu-id="b8aec-125">Az, hogy felvegye LMS az Azure ad integrálása megadásához kell hozzáadnia LMS felvegye a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="b8aec-125">To configure the integration of Absorb LMS in to Azure AD, you need to add Absorb LMS from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b8aec-126">**Adja hozzá a LMS felvegye a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b8aec-126">**To add Absorb LMS from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b8aec-127">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b8aec-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="b8aec-129">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b8aec-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b8aec-130">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b8aec-130">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="b8aec-132">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="b8aec-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="b8aec-134">Írja be a keresőmezőbe, **felvegye LMS**, jelölje be **felvegye LMS** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b8aec-134">In the search box, type **Absorb LMS**, select **Absorb LMS** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában LMS felvegye](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="b8aec-136">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b8aec-136">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="b8aec-137">Ebben a szakaszban konfigurálhatja, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján felvegye LMS</span><span class="sxs-lookup"><span data-stu-id="b8aec-137">In this section, you configure and test Azure AD single sign-on with Absorb LMS based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b8aec-138">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó LMS felvegye a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="b8aec-138">For single sign-on to work, Azure AD needs to know what the counterpart user in Absorb LMS is to a user in Azure AD.</span></span> <span data-ttu-id="b8aec-139">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a felvegye LMS közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="b8aec-139">In other words, a link relationship between an Azure AD user and the related user in Absorb LMS needs to be established.</span></span>

<span data-ttu-id="b8aec-140">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** LMS felvegye a.</span><span class="sxs-lookup"><span data-stu-id="b8aec-140">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Absorb LMS.</span></span>

<span data-ttu-id="b8aec-141">Az Azure AD egyszeri bejelentkezést a felvegye LMS tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="b8aec-141">To configure and test Azure AD single sign-on with Absorb LMS, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b8aec-142">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="b8aec-142">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b8aec-143">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="b8aec-143">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b8aec-144">**[Hozzon létre egy felvegye LMS tesztfelhasználó](#create-an-absorb-lms-test-user)**  - való egy megfelelője a Britta Simon LMS felvegye, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="b8aec-144">**[Create an Absorb LMS test user](#create-an-absorb-lms-test-user)** - to have a counterpart of Britta Simon in Absorb LMS that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b8aec-145">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b8aec-145">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b8aec-146">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="b8aec-146">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="b8aec-147">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b8aec-147">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="b8aec-148">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az felvegye LMS alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b8aec-148">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Absorb LMS application.</span></span>

<span data-ttu-id="b8aec-149">**Konfigurálása az Azure AD az egyszeri bejelentkezés LMS felvegye, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b8aec-149">**To configure Azure AD single sign-on with Absorb LMS, perform the following steps:**</span></span>

1. <span data-ttu-id="b8aec-150">Az Azure portálon a a **felvegye LMS** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b8aec-150">In the Azure portal, on the **Absorb LMS** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="b8aec-152">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b8aec-152">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_samlbase.png)

3. <span data-ttu-id="b8aec-154">Az a **felvegye LMS tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="b8aec-154">On the **Absorb LMS Domain and URLs** section, perform the following steps:</span></span>

    ![Felvegye LMS tartomány és az URL-címek egyetlen bejelentkezés információk](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_url.png)

    <span data-ttu-id="b8aec-156">a.</span><span class="sxs-lookup"><span data-stu-id="b8aec-156">a.</span></span> <span data-ttu-id="b8aec-157">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.myabsorb.com/Account/SAML`</span><span class="sxs-lookup"><span data-stu-id="b8aec-157">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.myabsorb.com/Account/SAML`</span></span>

    <span data-ttu-id="b8aec-158">b.</span><span class="sxs-lookup"><span data-stu-id="b8aec-158">b.</span></span> <span data-ttu-id="b8aec-159">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.myabsorb.com/Account/SAML`</span><span class="sxs-lookup"><span data-stu-id="b8aec-159">In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.myabsorb.com/Account/SAML`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="b8aec-160">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="b8aec-160">These values are not the real.</span></span> <span data-ttu-id="b8aec-161">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="b8aec-161">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="b8aec-162">Ügyfél [LMS ügyfél felvegye támogatási csoport](https://www.absorblms.com/support) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b8aec-162">Contact [Absorb LMS Client support team](https://www.absorblms.com/support) to get these values.</span></span> 

4. <span data-ttu-id="b8aec-163">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b8aec-163">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_certificate.png) 

6. <span data-ttu-id="b8aec-165">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b8aec-165">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-absorblms-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="b8aec-167">A a **felvegye LMS konfigurációs** kattintson **felvegye LMS konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="b8aec-167">On the **Absorb LMS Configuration** section, click **Configure Absorb LMS** to open **Configure sign-on** window.</span></span> <span data-ttu-id="b8aec-168">Másolás a **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="b8aec-168">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Felvegye LMS konfiguráció](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_configure.png) 

8. <span data-ttu-id="b8aec-170">Egy másik webes böngészőablakban jelentkezzen be a felvegye LMS vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b8aec-170">In a different web browser window, log in to your Absorb LMS company site as an administrator.</span></span>

9. <span data-ttu-id="b8aec-171">Kattintson a **fiók ikon** rendszergazdai kapcsolaton.</span><span class="sxs-lookup"><span data-stu-id="b8aec-171">Click the **Account Icon** on the admin interface.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/1.png)

10. <span data-ttu-id="b8aec-173">Kattintson a **portálbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="b8aec-173">Click **Portal Settings**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/2.png)
    
11. <span data-ttu-id="b8aec-175">Kattintson a **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="b8aec-175">Click the **Users** tab.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/3.png)

12. <span data-ttu-id="b8aec-177">Hajtsa végre a következő lépéseket az egyszeri bejelentkezés konfigurációs mezők eléréséhez:</span><span class="sxs-lookup"><span data-stu-id="b8aec-177">Perform the following steps to access the Single Sign-On configuration fields:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/4.png)

    <span data-ttu-id="b8aec-179">a.</span><span class="sxs-lookup"><span data-stu-id="b8aec-179">a.</span></span> <span data-ttu-id="b8aec-180">Válassza ki a megfelelő **mód**.</span><span class="sxs-lookup"><span data-stu-id="b8aec-180">Select the appropriate **Mode**.</span></span>

    <span data-ttu-id="b8aec-181">b.</span><span class="sxs-lookup"><span data-stu-id="b8aec-181">b.</span></span> <span data-ttu-id="b8aec-182">Nyissa meg a Jegyzettömbben, Azure-portálról letöltött tanúsítvány eltávolítása a **---BEGIN CERTIFICATE---** és **---vége tanúsítvány---** címkét, és illessze be a maradék tartalmat a **kulcs** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="b8aec-182">Open the Certificate that you have downloaded from the Azure portal in notepad, remove the **---BEGIN CERTIFICATE---** and **---END CERTIFICATE---** tag and then paste the remaining content in the **Key** textbox.</span></span>
    
    <span data-ttu-id="b8aec-183">c.</span><span class="sxs-lookup"><span data-stu-id="b8aec-183">c.</span></span> <span data-ttu-id="b8aec-184">Az a **azonosítóját megadó tulajdonságot**, válassza ki a megfelelő attribútumot, amely már konfigurálta a felhasználói azonosítóként (például, ha a userprinciplename az Azure AD-van kiválasztva, majd a felhasználónév itt választott lenne.) az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="b8aec-184">In the **Id Property**, select the appropriate attribute which you have configured as the user identifier in the Azure AD (For example, If the userprinciplename is selected in Azure AD, then Username would be selected here.)</span></span>

    <span data-ttu-id="b8aec-185">d.</span><span class="sxs-lookup"><span data-stu-id="b8aec-185">d.</span></span> <span data-ttu-id="b8aec-186">Az a **bejelentkezési URL-cím**, illessze be a **"SAML-alapú egyszeri bejelentkezési URL-címe"** másolta az érték a **bejelentkezés konfigurálása** ablakot, az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="b8aec-186">In the **Login URL**, paste the **“SAML Single Sign-On Service URL”** value you have copied from the **Configure sign-on** window of the Azure portal.</span></span>

    <span data-ttu-id="b8aec-187">e.</span><span class="sxs-lookup"><span data-stu-id="b8aec-187">e.</span></span> <span data-ttu-id="b8aec-188">Az a **kijelentkezési URL-cím**, illessze be a **"Sign-Out URL-címe"** másolta az érték a **bejelentkezés konfigurálása** ablakot, az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="b8aec-188">In the **Logout URL**, paste the **“Sign-Out URL”** value you have copied from the **Configure sign-on** window of the Azure portal.</span></span>

13. <span data-ttu-id="b8aec-189">Engedélyezése **"Csak engedélyezése Egyszeri bejelentkezéshez"**.</span><span class="sxs-lookup"><span data-stu-id="b8aec-189">Enable **‘Only Allow SSO Login’**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/5.png)

14. <span data-ttu-id="b8aec-191">Kattintson a **"Mentés".**</span><span class="sxs-lookup"><span data-stu-id="b8aec-191">Click **"Save."**</span></span>

> [!TIP]
> <span data-ttu-id="b8aec-192">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="b8aec-192">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b8aec-193">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="b8aec-193">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b8aec-194">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b8aec-194">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="b8aec-195">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="b8aec-195">Create an Azure AD test user</span></span>

<span data-ttu-id="b8aec-196">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="b8aec-196">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="b8aec-198">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b8aec-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b8aec-199">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b8aec-199">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-absorblms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b8aec-201">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="b8aec-201">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-absorblms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b8aec-203">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b8aec-203">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![A Hozzáadás gombra.](./media/active-directory-saas-absorblms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b8aec-205">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="b8aec-205">On the **User** dialog page, perform the following steps:</span></span>
 
    ![A felhasználó párbeszédpanel](./media/active-directory-saas-absorblms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b8aec-207">a.</span><span class="sxs-lookup"><span data-stu-id="b8aec-207">a.</span></span> <span data-ttu-id="b8aec-208">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b8aec-208">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b8aec-209">b.</span><span class="sxs-lookup"><span data-stu-id="b8aec-209">b.</span></span> <span data-ttu-id="b8aec-210">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b8aec-210">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b8aec-211">c.</span><span class="sxs-lookup"><span data-stu-id="b8aec-211">c.</span></span> <span data-ttu-id="b8aec-212">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="b8aec-212">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b8aec-213">d.</span><span class="sxs-lookup"><span data-stu-id="b8aec-213">d.</span></span> <span data-ttu-id="b8aec-214">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b8aec-214">Click **Create**.</span></span>

### <a name="create-an-absorb-lms-test-user"></a><span data-ttu-id="b8aec-215">Hozzon létre egy felvegye LMS tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="b8aec-215">Create an Absorb LMS test user</span></span>

<span data-ttu-id="b8aec-216">Ahhoz, hogy felvegye LMS jelentkezzenek be az Azure AD-felhasználók, akkor ki kell építenie a LMS befogadására.</span><span class="sxs-lookup"><span data-stu-id="b8aec-216">To enable Azure AD users to log in to Absorb LMS, they must be provisioned in to Absorb LMS.</span></span>  
<span data-ttu-id="b8aec-217">LMS felvegye a kiépítés kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="b8aec-217">For Absorb LMS, provisioning is a manual task.</span></span>

<span data-ttu-id="b8aec-218">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b8aec-218">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="b8aec-219">Jelentkezzen be rendszergazdaként a felvegye LMS vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="b8aec-219">Log in to your Absorb LMS company site as an administrator.</span></span>

2. <span data-ttu-id="b8aec-220">Kattintson a **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="b8aec-220">Click **Users** tab.</span></span>

    ![Felkérése](./media/active-directory-saas-absorblms-tutorial/absorblms_users.png)

3. <span data-ttu-id="b8aec-222">Kattintson a **felhasználók** alatt a **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="b8aec-222">Click **Users** under the **Users** tab.</span></span>

    ![Felkérése](./media/active-directory-saas-absorblms-tutorial/absorblms_userssub.png)

4.  <span data-ttu-id="b8aec-224">Válassza ki **felhasználói** a **új hozzáadása** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="b8aec-224">Select **User** from **Add New** drop-down.</span></span>

    ![Felkérése](./media/active-directory-saas-absorblms-tutorial/absorblms_createuser.png)

5. <span data-ttu-id="b8aec-226">Az a **felhasználó hozzáadása** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="b8aec-226">On the **Add User** page, perform the following steps:</span></span>

    ![Felkérése](./media/active-directory-saas-absorblms-tutorial/user.png)

    <span data-ttu-id="b8aec-228">a.</span><span class="sxs-lookup"><span data-stu-id="b8aec-228">a.</span></span> <span data-ttu-id="b8aec-229">Az a **Keresztnév** szövegmezőben, az első típusnév Britta hasonlóan.</span><span class="sxs-lookup"><span data-stu-id="b8aec-229">In the **First Name** textbox, type the first name like Britta.</span></span>

    <span data-ttu-id="b8aec-230">b.</span><span class="sxs-lookup"><span data-stu-id="b8aec-230">b.</span></span> <span data-ttu-id="b8aec-231">Az a **Vezetéknév** szövegmező, írja be például a Simon utolsó nevét.</span><span class="sxs-lookup"><span data-stu-id="b8aec-231">In the **Last Name** textbox, type the last name like Simon.</span></span>
    
    <span data-ttu-id="b8aec-232">c.</span><span class="sxs-lookup"><span data-stu-id="b8aec-232">c.</span></span> <span data-ttu-id="b8aec-233">Az a **felhasználónév** szövegmező, írja be a felhasználónevet, például a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b8aec-233">In the **Username** textbox, type the user name like Britta Simon.</span></span>

    <span data-ttu-id="b8aec-234">d.</span><span class="sxs-lookup"><span data-stu-id="b8aec-234">d.</span></span> <span data-ttu-id="b8aec-235">Az a **jelszó** szövegmező, írja be a Britta Simon tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="b8aec-235">In the **Password** textbox, type the password of Britta Simon.</span></span>

    <span data-ttu-id="b8aec-236">e.</span><span class="sxs-lookup"><span data-stu-id="b8aec-236">e.</span></span> <span data-ttu-id="b8aec-237">Az a **jelszó megerősítése** szövegmezőhöz ugyanazt a jelszót.</span><span class="sxs-lookup"><span data-stu-id="b8aec-237">In the **Confirm Password** textbox, type the same password.</span></span>
    
    <span data-ttu-id="b8aec-238">f.</span><span class="sxs-lookup"><span data-stu-id="b8aec-238">f.</span></span> <span data-ttu-id="b8aec-239">Ügyeljen rá **aktív**.</span><span class="sxs-lookup"><span data-stu-id="b8aec-239">Make it as **ACTIVE**.</span></span>   

6. <span data-ttu-id="b8aec-240">Kattintson a **"Mentés".**</span><span class="sxs-lookup"><span data-stu-id="b8aec-240">Click **"Save."**</span></span>
 
### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="b8aec-241">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="b8aec-241">Assign the Azure AD test user</span></span>

<span data-ttu-id="b8aec-242">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés felvegye LMS Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="b8aec-242">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Absorb LMS.</span></span>

![A felhasználói szerepkör hozzárendelése][200]

<span data-ttu-id="b8aec-244">**Britta Simon hozzárendelése LMS felvegye, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b8aec-244">**To assign Britta Simon to Absorb LMS, perform the following steps:**</span></span>

1. <span data-ttu-id="b8aec-245">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b8aec-245">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b8aec-247">Az alkalmazások listában válassza ki a **felvegye LMS**.</span><span class="sxs-lookup"><span data-stu-id="b8aec-247">In the applications list, select **Absorb LMS**.</span></span>

    ![Az alkalmazások listáját a felvegye LMS hivatkozás](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_app.png) 

3. <span data-ttu-id="b8aec-249">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b8aec-249">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202] 

4. <span data-ttu-id="b8aec-251">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b8aec-251">Click **Add** button.</span></span> <span data-ttu-id="b8aec-252">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b8aec-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="b8aec-254">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b8aec-254">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b8aec-255">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b8aec-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b8aec-256">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b8aec-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="b8aec-257">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b8aec-257">Test single sign-on</span></span>

<span data-ttu-id="b8aec-258">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="b8aec-258">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b8aec-259">Kattintson a hozzáférési panelen felvegye LMS csempére, meg fogja lekérni automatikusan bejelentkezett felvegye LMS Alkalmazásmódosítások.</span><span class="sxs-lookup"><span data-stu-id="b8aec-259">Click the Absorb LMS tile in the Access Panel, you will get automatically signed-on to your Absorb LMS application.</span></span> <span data-ttu-id="b8aec-260">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="b8aec-260">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b8aec-261">További források</span><span class="sxs-lookup"><span data-stu-id="b8aec-261">Additional resources</span></span>

* [<span data-ttu-id="b8aec-262">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="b8aec-262">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b8aec-263">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b8aec-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_203.png

