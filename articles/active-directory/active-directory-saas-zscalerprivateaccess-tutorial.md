---
title: "Oktatóanyag: Azure Active Directory-integrációval rendelkező Zscaler személyes hozzáférési (ZPA) |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Zscaler személyes hozzáférési (ZPA) között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 83711115-1c4f-4dd7-907b-3da24b37c89e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.openlocfilehash: 5c598bfa5b6725d21a89df54dbcb3314cc631d80
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-zpa"></a><span data-ttu-id="b396d-103">Oktatóanyag: Azure Active Directory-integrációval rendelkező Zscaler személyes hozzáférési (ZPA)</span><span class="sxs-lookup"><span data-stu-id="b396d-103">Tutorial: Azure Active Directory integration with Zscaler Private Access (ZPA)</span></span>

<span data-ttu-id="b396d-104">Ebben az oktatóanyagban elsajátíthatja Zscaler személyes hozzáférési (ZPA) integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b396d-104">In this tutorial, you learn how to integrate Zscaler Private Access (ZPA) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b396d-105">Zscaler személyes hozzáférési (ZPA) integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="b396d-105">Integrating Zscaler Private Access (ZPA) with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b396d-106">Szabályozhatja az Azure AD, aki hozzáfér a Zscaler személyes hozzáférési (ZPA)</span><span class="sxs-lookup"><span data-stu-id="b396d-106">You can control in Azure AD who has access to Zscaler Private Access (ZPA)</span></span>
- <span data-ttu-id="b396d-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett a Zscaler személyes hozzáférési (ZPA) (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="b396d-107">You can enable your users to automatically get signed-on to Zscaler Private Access (ZPA) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b396d-108">Kezelheti a fiókokat, egy központi helyen – az Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="b396d-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="b396d-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b396d-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b396d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b396d-110">Prerequisites</span></span>

<span data-ttu-id="b396d-111">Az Azure AD-integráció konfigurálása a Zscaler személyes hozzáférési (ZPA), a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="b396d-111">To configure Azure AD integration with Zscaler Private Access (ZPA), you need the following items:</span></span>

- <span data-ttu-id="b396d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b396d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b396d-113">Egy Zscaler személyes hozzáférési (ZPA) egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="b396d-113">A Zscaler Private Access (ZPA) single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="b396d-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="b396d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="b396d-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="b396d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b396d-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b396d-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="b396d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b396d-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="b396d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b396d-118">Scenario description</span></span>
<span data-ttu-id="b396d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b396d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b396d-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b396d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b396d-121">Zscaler személyes hozzáférési (ZPA) hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="b396d-121">Adding Zscaler Private Access (ZPA) from the gallery</span></span>
2. <span data-ttu-id="b396d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b396d-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-zscaler-private-access-zpa-from-the-gallery"></a><span data-ttu-id="b396d-123">Zscaler személyes hozzáférési (ZPA) hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="b396d-123">Adding Zscaler Private Access (ZPA) from the gallery</span></span>
<span data-ttu-id="b396d-124">Az Azure AD integrálása a Zscaler személyes hozzáférési (ZPA) konfigurálásához kell hozzáadnia Zscaler személyes hozzáférési (ZPA) a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="b396d-124">To configure the integration of Zscaler Private Access (ZPA) into Azure AD, you need to add Zscaler Private Access (ZPA) from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b396d-125">**A gyűjteményből Zscaler személyes hozzáférési (ZPA) hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b396d-125">**To add Zscaler Private Access (ZPA) from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b396d-126">Az a  **[Azure felügyeleti portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b396d-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b396d-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b396d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b396d-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b396d-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="b396d-131">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="b396d-131">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="b396d-133">Írja be a keresőmezőbe, **Zscaler személyes hozzáférési (ZPA)**.</span><span class="sxs-lookup"><span data-stu-id="b396d-133">In the search box, type **Zscaler Private Access (ZPA)**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_001.png)

5. <span data-ttu-id="b396d-135">Az eredmények panelen válassza ki a **Zscaler személyes hozzáférési (ZPA)**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b396d-135">In the results panel, select **Zscaler Private Access (ZPA)**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b396d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b396d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b396d-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Zscaler személyes hozzáférési (ZPA) "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="b396d-138">In this section, you configure and test Azure AD single sign-on with Zscaler Private Access (ZPA) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b396d-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó a Zscaler személyes hozzáférési (ZPA) a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="b396d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Zscaler Private Access (ZPA) is to a user in Azure AD.</span></span> <span data-ttu-id="b396d-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Zscaler személyes hozzáférési (ZPA) közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="b396d-140">In other words, a link relationship between an Azure AD user and the related user in Zscaler Private Access (ZPA) needs to be established.</span></span>

<span data-ttu-id="b396d-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a Zscaler személyes hozzáférési (ZPA).</span><span class="sxs-lookup"><span data-stu-id="b396d-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Zscaler Private Access (ZPA).</span></span>

<span data-ttu-id="b396d-142">Az Azure AD egyszeri bejelentkezést a Zscaler személyes hozzáférési (ZPA) tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="b396d-142">To configure and test Azure AD single sign-on with Zscaler Private Access (ZPA), you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b396d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="b396d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b396d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="b396d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b396d-145">**[Zscaler személyes hozzáférési (ZPA) tesztfelhasználó létrehozása](#creating-a-zscaler-private-access-(zpa)-test-user)**  - kell rendelkeznie a megfelelője a Britta Simon a Zscaler személyes hozzáférési (ZPA), amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="b396d-145">**[Creating a Zscaler Private Access (ZPA) test user](#creating-a-zscaler-private-access-(zpa)-test-user)** - to have a counterpart of Britta Simon in Zscaler Private Access (ZPA) that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="b396d-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b396d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b396d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="b396d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b396d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b396d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b396d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Zscaler személyes hozzáférési (ZPA) alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b396d-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Zscaler Private Access (ZPA) application.</span></span>

<span data-ttu-id="b396d-150">**Az Azure AD az egyszeri bejelentkezés beállítása a Zscaler személyes hozzáférési (ZPA), hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b396d-150">**To configure Azure AD single sign-on with Zscaler Private Access (ZPA), perform the following steps:**</span></span>

1. <span data-ttu-id="b396d-151">Az Azure felügyeleti portálján a a **Zscaler személyes hozzáférési (ZPA)** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b396d-151">In the Azure Management portal, on the **Zscaler Private Access (ZPA)** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="b396d-153">A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="b396d-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_300.png)
    
3. <span data-ttu-id="b396d-155">Az a **Zscaler személyes hozzáférési (ZPA) tartományhoz és URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="b396d-155">On the **Zscaler Private Access (ZPA) Domain and URLs** section, perform the following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_01.png)

    <span data-ttu-id="b396d-157">a.</span><span class="sxs-lookup"><span data-stu-id="b396d-157">a.</span></span> <span data-ttu-id="b396d-158">Az a **URL-cím bejelentkezési** szövegmező, adja meg a következő minta használatával URL-címe:`https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`</span><span class="sxs-lookup"><span data-stu-id="b396d-158">In the **Sign On URL** textbox, type a URL using the following pattern: `https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`</span></span>

    <span data-ttu-id="b396d-159">b.</span><span class="sxs-lookup"><span data-stu-id="b396d-159">b.</span></span> <span data-ttu-id="b396d-160">Az a **azonosító** szövegmező, típus:`https://samlsp.private.zscaler.com/auth/metadata`</span><span class="sxs-lookup"><span data-stu-id="b396d-160">In the **Identifier** textbox, type: `https://samlsp.private.zscaler.com/auth/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b396d-161">Ne feledje, hogy ezek nincsenek a valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="b396d-161">Please note that these are not the real values.</span></span> <span data-ttu-id="b396d-162">Akkor frissítheti ezeket az értékeket, és a tényleges URL-cím bejelentkezési azonosítója.</span><span class="sxs-lookup"><span data-stu-id="b396d-162">You have to update these values with the actual Sign On URL and Identifier.</span></span> <span data-ttu-id="b396d-163">Itt javasoljuk, hogy az azonosító URL-címe egyedi értékét használja.</span><span class="sxs-lookup"><span data-stu-id="b396d-163">Here we suggest you to use the unique value of URL in the Identifier.</span></span> <span data-ttu-id="b396d-164">Ügyfél [Zscaler személyes hozzáférési (ZPA) támogatási csoport](https://help.zscaler.com/zpa-submit-ticket) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b396d-164">Contact [Zscaler Private Access (ZPA) support team](https://help.zscaler.com/zpa-submit-ticket) to get these values.</span></span>

4. <span data-ttu-id="b396d-165">Az a **SAML-aláíró tanúsítványa** kattintson **hozzon létre új tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="b396d-165">On the **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_400.png)   

5. <span data-ttu-id="b396d-167">A a **új tanúsítvány létrehozása** párbeszédpanel, kattintson a naptár ikonra, és válasszon egy **lejárati dátum**.</span><span class="sxs-lookup"><span data-stu-id="b396d-167">On the **Create New Certificate** dialog, click the calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="b396d-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b396d-168">Then click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_500.png)

6. <span data-ttu-id="b396d-170">Az a **SAML-aláíró tanúsítványa** szakaszban jelölje be **új tanúsítvány aktiválásához** kattintson **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b396d-170">On the **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_02.png)

7. <span data-ttu-id="b396d-172">Az előugró **helyettesítő tanúsítvány** ablak, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="b396d-172">On the pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_600.png)

8. <span data-ttu-id="b396d-174">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b396d-174">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_03.png) 

9. <span data-ttu-id="b396d-176">Egy másik webes böngészőablakban jelentkezzen be a Zscaler személyes hozzáférési (ZPA) vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b396d-176">In a different web browser window, log into your Zscaler Private Access (ZPA) company site as an administrator.</span></span>

10. <span data-ttu-id="b396d-177">Navigáljon a **rendszergazda** majd **Idp konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="b396d-177">Navigate to **Administrator** and then click **Idp Configuration**.</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_04.png)

11. <span data-ttu-id="b396d-179">Az a **Idp konfigurációs** kattintson **hozzáadása új IDP konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="b396d-179">In the **Idp Configuration** section, click **Add New IDP Configuration**.</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_05.png)

12. <span data-ttu-id="b396d-181">Az a **új IDP konfigurációs** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="b396d-181">In the **New IDP Configuration** section, perform the following steps:</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_06.png)

    <span data-ttu-id="b396d-183">a.</span><span class="sxs-lookup"><span data-stu-id="b396d-183">a.</span></span> <span data-ttu-id="b396d-184">Kattintson a **fájl kiválasztása** , és töltse fel a letöltött metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="b396d-184">Click **Select File** and upload your downloaded metadata file.</span></span>

    <span data-ttu-id="b396d-185">b.</span><span class="sxs-lookup"><span data-stu-id="b396d-185">b.</span></span> <span data-ttu-id="b396d-186">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b396d-186">Click **Save** button.</span></span>
    


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b396d-187">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b396d-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="b396d-188">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure felügyeleti portálján Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="b396d-188">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="b396d-190">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b396d-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b396d-191">Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b396d-191">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b396d-193">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b396d-193">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b396d-195">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b396d-195">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b396d-197">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="b396d-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b396d-199">a.</span><span class="sxs-lookup"><span data-stu-id="b396d-199">a.</span></span> <span data-ttu-id="b396d-200">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b396d-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b396d-201">b.</span><span class="sxs-lookup"><span data-stu-id="b396d-201">b.</span></span> <span data-ttu-id="b396d-202">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b396d-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b396d-203">c.</span><span class="sxs-lookup"><span data-stu-id="b396d-203">c.</span></span> <span data-ttu-id="b396d-204">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="b396d-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b396d-205">d.</span><span class="sxs-lookup"><span data-stu-id="b396d-205">d.</span></span> <span data-ttu-id="b396d-206">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b396d-206">Click **Create**.</span></span> 



### <a name="creating-a-zscaler-private-access-zpa-test-user"></a><span data-ttu-id="b396d-207">Zscaler személyes hozzáférési (ZPA) tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b396d-207">Creating a Zscaler Private Access (ZPA) test user</span></span>

<span data-ttu-id="b396d-208">Ebben a szakaszban egy Britta Simon a Zscaler személyes hozzáférési (ZPA) nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b396d-208">In this section, you create a user called Britta Simon in Zscaler Private Access (ZPA).</span></span> <span data-ttu-id="b396d-209">Adjon együttműködve [Zscaler személyes hozzáférési (ZPA) támogatási csoport](https://help.zscaler.com/zpa-submit-ticket) a felhasználók hozzáadása a Zscaler személyes hozzáférési (ZPA) platform.</span><span class="sxs-lookup"><span data-stu-id="b396d-209">Please work with [Zscaler Private Access (ZPA) support team](https://help.zscaler.com/zpa-submit-ticket) to add the users in the Zscaler Private Access (ZPA) platform.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b396d-210">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b396d-210">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b396d-211">Ebben a szakaszban Britta Simon saját hozzáférés biztosítása a Zscaler személyes hozzáférési (ZPA) által használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b396d-211">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Zscaler Private Access (ZPA).</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="b396d-213">**A Zscaler személyes hozzáférési (ZPA) Britta Simon hozzárendeléséhez a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="b396d-213">**To assign Britta Simon to Zscaler Private Access (ZPA), perform the following steps:**</span></span>

1. <span data-ttu-id="b396d-214">Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b396d-214">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b396d-216">Az alkalmazások listában válassza ki a **Zscaler személyes hozzáférési (ZPA)**.</span><span class="sxs-lookup"><span data-stu-id="b396d-216">In the applications list, select **Zscaler Private Access (ZPA)**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_50.png) 

3. <span data-ttu-id="b396d-218">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b396d-218">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="b396d-220">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b396d-220">Click **Add** button.</span></span> <span data-ttu-id="b396d-221">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b396d-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="b396d-223">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b396d-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b396d-224">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b396d-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b396d-225">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b396d-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="b396d-226">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b396d-226">Testing single sign-on</span></span>

<span data-ttu-id="b396d-227">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="b396d-227">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b396d-228">Ha a hozzáférési panelen Zscaler személyes hozzáférési (ZPA) csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Zscaler személyes hozzáférési (ZPA) alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="b396d-228">When you click the Zscaler Private Access (ZPA) tile in the Access Panel, you should get automatically signed-on to your Zscaler Private Access (ZPA) application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="b396d-229">További források</span><span class="sxs-lookup"><span data-stu-id="b396d-229">Additional resources</span></span>

* [<span data-ttu-id="b396d-230">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="b396d-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b396d-231">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b396d-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_203.png