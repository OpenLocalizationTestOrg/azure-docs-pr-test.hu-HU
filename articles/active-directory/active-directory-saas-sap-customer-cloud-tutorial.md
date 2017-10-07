---
title: "Oktatóanyag: Azure Active Directory-integráció a SAP felhőalapú ügyfél |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az ügyfél SAP felhő között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 0525ea81122458ab3ac24a5bdb0b5f628405dd05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a><span data-ttu-id="5d3be-103">Oktatóanyag: Azure Active Directory-integráció a SAP felhőalapú ügyfél</span><span class="sxs-lookup"><span data-stu-id="5d3be-103">Tutorial: Azure Active Directory integration with SAP Cloud for Customer</span></span>

<span data-ttu-id="5d3be-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate SAP felhőalapú Azure Active Directory (Azure AD-) ügyfél.</span><span class="sxs-lookup"><span data-stu-id="5d3be-104">In this tutorial, you learn how toointegrate SAP Cloud for Customer with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5d3be-105">SAP felhő ügyfél és az Azure AD integrálása lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="5d3be-105">Integrating SAP Cloud for Customer with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5d3be-106">Szabályozhatja az Azure AD hozzáférési tooSAP felhő rendelkező ügyfél</span><span class="sxs-lookup"><span data-stu-id="5d3be-106">You can control in Azure AD who has access tooSAP Cloud for Customer</span></span>
- <span data-ttu-id="5d3be-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSAP felhő ügyfél (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="5d3be-107">You can enable your users tooautomatically get signed-on tooSAP Cloud for Customer (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5d3be-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5d3be-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5d3be-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5d3be-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5d3be-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5d3be-110">Prerequisites</span></span>

<span data-ttu-id="5d3be-111">ügyfél a SAP felhőalapú Azure AD-integrációs tooconfigure, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="5d3be-111">tooconfigure Azure AD integration with SAP Cloud for Customer, you need hello following items:</span></span>

- <span data-ttu-id="5d3be-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="5d3be-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5d3be-113">Az ügyfél egyszeri bejelentkezés SAP felhő előfizetés engedélyezve</span><span class="sxs-lookup"><span data-stu-id="5d3be-113">A SAP Cloud for Customer single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5d3be-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="5d3be-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5d3be-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="5d3be-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5d3be-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="5d3be-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5d3be-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5d3be-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5d3be-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="5d3be-118">Scenario description</span></span>
<span data-ttu-id="5d3be-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="5d3be-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5d3be-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="5d3be-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5d3be-121">Ügyfél SAP felhő felvételekor hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="5d3be-121">Adding SAP Cloud for Customer from hello gallery</span></span>
2. <span data-ttu-id="5d3be-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5d3be-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sap-cloud-for-customer-from-hello-gallery"></a><span data-ttu-id="5d3be-123">Ügyfél SAP felhő felvételekor hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="5d3be-123">Adding SAP Cloud for Customer from hello gallery</span></span>
<span data-ttu-id="5d3be-124">tooconfigure hello integrációs SAP felhő ügyfél az Azure AD-be, szükség van tooadd SAP felhő ügyfél hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="5d3be-124">tooconfigure hello integration of SAP Cloud for Customer into Azure AD, you need tooadd SAP Cloud for Customer from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5d3be-125">**tooadd SAP felhő ügyfél hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="5d3be-125">**tooadd SAP Cloud for Customer from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5d3be-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5d3be-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5d3be-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="5d3be-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5d3be-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5d3be-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="5d3be-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="5d3be-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="5d3be-133">Hello keresési mezőbe, írja be a **SAP felhő ügyfél**.</span><span class="sxs-lookup"><span data-stu-id="5d3be-133">In hello search box, type **SAP Cloud for Customer**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_search.png)

5. <span data-ttu-id="5d3be-135">Hello eredmények panelen, jelölje ki a **SAP felhő ügyfél**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="5d3be-135">In hello results panel, select **SAP Cloud for Customer**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5d3be-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5d3be-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5d3be-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez a SAP felhőalapú ügyfél "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="5d3be-138">In this section, you configure and test Azure AD single sign-on with SAP Cloud for Customer based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5d3be-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói SAP felhőben ügyfél tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="5d3be-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SAP Cloud for Customer is tooa user in Azure AD.</span></span> <span data-ttu-id="5d3be-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello SAP felhőben ügyfél közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="5d3be-140">In other words, a link relationship between an Azure AD user and hello related user in SAP Cloud for Customer needs toobe established.</span></span>

<span data-ttu-id="5d3be-141">Ügyfél SAP felhőben rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="5d3be-141">In SAP Cloud for Customer, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5d3be-142">tooconfigure és tesztelése az Azure AD egyszeri bejelentkezést a SAP felhőalapú ügyfél, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="5d3be-142">tooconfigure and test Azure AD single sign-on with SAP Cloud for Customer, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5d3be-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="5d3be-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5d3be-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5d3be-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5d3be-145">**[Az ügyfél tesztfelhasználó SAP felhők létrehozásával](#creating-a-sap-cloud-for-customer-test-user)**  -toohave egy megfelelője a Britta Simon SAP felhőben ügyfél, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="5d3be-145">**[Creating a SAP Cloud for Customer test user](#creating-a-sap-cloud-for-customer-test-user)** - toohave a counterpart of Britta Simon in SAP Cloud for Customer that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5d3be-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="5d3be-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5d3be-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="5d3be-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5d3be-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5d3be-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5d3be-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az SAP-felhőben felhasználói alkalmazás egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="5d3be-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SAP Cloud for Customer application.</span></span>

<span data-ttu-id="5d3be-150">**tooconfigure az Azure AD egyszeri bejelentkezést a SAP felhőalapú ügyfél, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="5d3be-150">**tooconfigure Azure AD single sign-on with SAP Cloud for Customer, perform hello following steps:**</span></span>

1. <span data-ttu-id="5d3be-151">Az Azure portál, a hello hello **SAP felhő ügyfél** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="5d3be-151">In hello Azure portal, on hello **SAP Cloud for Customer** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="5d3be-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="5d3be-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_samlbase.png)

3. <span data-ttu-id="5d3be-155">A hello **SAP felhő a felhasználói tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5d3be-155">On hello **SAP Cloud for Customer Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_url.png)

    <span data-ttu-id="5d3be-157">a.</span><span class="sxs-lookup"><span data-stu-id="5d3be-157">a.</span></span> <span data-ttu-id="5d3be-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server name>.crm.ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="5d3be-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<server name>.crm.ondemand.com`</span></span>

    <span data-ttu-id="5d3be-159">b.</span><span class="sxs-lookup"><span data-stu-id="5d3be-159">b.</span></span> <span data-ttu-id="5d3be-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server name>.crm.ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="5d3be-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<server name>.crm.ondemand.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5d3be-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="5d3be-161">These values are not real.</span></span> <span data-ttu-id="5d3be-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="5d3be-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="5d3be-163">Ügyfél [SAP felhő ügyfél ügyfél-támogatási csoport](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="5d3be-163">Contact [SAP Cloud for Customer Client support team](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) tooget these values.</span></span> 

4. <span data-ttu-id="5d3be-164">A hello **felhasználói attribútumok** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5d3be-164">On hello **User Attributes** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_attribute.png)

    <span data-ttu-id="5d3be-166">a.</span><span class="sxs-lookup"><span data-stu-id="5d3be-166">a.</span></span> <span data-ttu-id="5d3be-167">A **felhasználói azonosító** listában, jelölje be hello **ExtractMailPrefix()** függvény.</span><span class="sxs-lookup"><span data-stu-id="5d3be-167">In **User Identifier** list, select hello **ExtractMailPrefix()** function.</span></span>

    <span data-ttu-id="5d3be-168">b.</span><span class="sxs-lookup"><span data-stu-id="5d3be-168">b.</span></span> <span data-ttu-id="5d3be-169">A hello **Mail** listán, válassza hello felhasználói attribútum a megvalósítás toouse használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="5d3be-169">From hello **Mail** list, select hello user attribute you want toouse for your implementation.</span></span>
    <span data-ttu-id="5d3be-170">Például ha azt szeretné, hogy az egyedi felhasználói azonosítóval EmployeeID toouse hello és hello ExtensionAttribute2 tárolt hello attribútum értéke, majd válassza ki user.extensionattribute2.</span><span class="sxs-lookup"><span data-stu-id="5d3be-170">For example, if you want toouse hello EmployeeID as unique user identifier and you have stored hello attribute value in hello ExtensionAttribute2, then select user.extensionattribute2.</span></span>  

5. <span data-ttu-id="5d3be-171">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5d3be-171">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_certificate.png) 

6. <span data-ttu-id="5d3be-173">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5d3be-173">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="5d3be-175">A hello **SAP felhő a felhasználói konfigurálása** kattintson **SAP felhő konfigurálása a felhasználói** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="5d3be-175">On hello **SAP Cloud for Customer Configuration** section, click **Configure SAP Cloud for Customer** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5d3be-176">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="5d3be-176">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_configure.png) 

8. <span data-ttu-id="5d3be-178">tooget SSO konfigurálva, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="5d3be-178">tooget SSO configured, perform hello following steps:</span></span>
   
    <span data-ttu-id="5d3be-179">a.</span><span class="sxs-lookup"><span data-stu-id="5d3be-179">a.</span></span> <span data-ttu-id="5d3be-180">Bejelentkezés SAP felhő a felhasználói portál rendszergazda jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="5d3be-180">Login into SAP Cloud for Customer portal with administrator rights.</span></span>
   
    <span data-ttu-id="5d3be-181">b.</span><span class="sxs-lookup"><span data-stu-id="5d3be-181">b.</span></span> <span data-ttu-id="5d3be-182">Keresse meg a toohello **alkalmazás- és felhasználói gyakori feladatot** hello kattintson **identitásszolgáltató** fülre.</span><span class="sxs-lookup"><span data-stu-id="5d3be-182">Navigate toohello **Application and User Management Common Task** and click hello **Identity Provider** tab.</span></span>
   
    <span data-ttu-id="5d3be-183">c.</span><span class="sxs-lookup"><span data-stu-id="5d3be-183">c.</span></span> <span data-ttu-id="5d3be-184">Kattintson a **új identitásszolgáltató** és select hello metaadatok XML-fájl már letöltötte az Azure-portálon hello.</span><span class="sxs-lookup"><span data-stu-id="5d3be-184">Click **New Identity Provider** and select hello metadata XML file you have downloaded from hello Azure portal.</span></span> <span data-ttu-id="5d3be-185">Hello metaadatok importálásával hello rendszer automatikusan feltölti a hello szükséges aláírási tanúsítvány és a titkosítási tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="5d3be-185">By importing hello metadata, hello system automatically uploads hello required signature certificate and encryption certificate.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_54.png)
   
    <span data-ttu-id="5d3be-187">d.</span><span class="sxs-lookup"><span data-stu-id="5d3be-187">d.</span></span> <span data-ttu-id="5d3be-188">Az Azure Active Directory hello SAML-kérelmet hello elem helyességi feltétel ügyfél szolgáltatás URL-címe szükséges, válassza ki a hello **helyességi feltétel fogyasztói szolgáltatás URL-címek** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="5d3be-188">Azure Active Directory requires hello element Assertion Consumer Service URL in hello SAML request, so select hello **Include Assertion Consumer Service URL** checkbox.</span></span>
   
    <span data-ttu-id="5d3be-189">e.</span><span class="sxs-lookup"><span data-stu-id="5d3be-189">e.</span></span> <span data-ttu-id="5d3be-190">Kattintson a **egyszeri bejelentkezés aktiválása**.</span><span class="sxs-lookup"><span data-stu-id="5d3be-190">Click **Activate Single Sign-On**.</span></span>
   
    <span data-ttu-id="5d3be-191">f.</span><span class="sxs-lookup"><span data-stu-id="5d3be-191">f.</span></span> <span data-ttu-id="5d3be-192">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="5d3be-192">Save your changes.</span></span>
   
    <span data-ttu-id="5d3be-193">g.</span><span class="sxs-lookup"><span data-stu-id="5d3be-193">g.</span></span> <span data-ttu-id="5d3be-194">Kattintson a hello **a rendszer** fülre.</span><span class="sxs-lookup"><span data-stu-id="5d3be-194">Click hello **My System** tab.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_52.png)
   
    <span data-ttu-id="5d3be-196">h.</span><span class="sxs-lookup"><span data-stu-id="5d3be-196">h.</span></span> <span data-ttu-id="5d3be-197">A **Azure AD bejelentkezési URL-címen** szövegmezőhöz Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="5d3be-197">In **Azure AD Sign On URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_53.png)
   
    <span data-ttu-id="5d3be-199">i.</span><span class="sxs-lookup"><span data-stu-id="5d3be-199">i.</span></span> <span data-ttu-id="5d3be-200">Adja meg, hogy hello alkalmazott manuálisan választhat naplózás a felhasználói Azonosítót és jelszót vagy az SSO hello kiválasztásával **manuális Identity Provider kijelölés**.</span><span class="sxs-lookup"><span data-stu-id="5d3be-200">Specify whether hello employee can manually choose between logging on with user ID and password or SSO by selecting hello **Manual Identity Provider Selection**.</span></span>
   
    <span data-ttu-id="5d3be-201">j.</span><span class="sxs-lookup"><span data-stu-id="5d3be-201">j.</span></span> <span data-ttu-id="5d3be-202">A hello **egyszeri bejelentkezési URL-cím** szakasz hello URL-cím, amelyet az alkalmazottak toosign toohello rendszeren kell használni.</span><span class="sxs-lookup"><span data-stu-id="5d3be-202">In hello **SSO URL** section, specify hello URL that should be used by your employees toosign on toohello system.</span></span> 
    <span data-ttu-id="5d3be-203">A hello **URL-cím küldött tooEmployee** lista, választhat a következő beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="5d3be-203">In hello **URL Sent tooEmployee** list, you can choose between hello following options:</span></span>
   
    <span data-ttu-id="5d3be-204">**Nem egyszeri bejelentkezési URL-címe**</span><span class="sxs-lookup"><span data-stu-id="5d3be-204">**Non-SSO URL**</span></span>
   
    <span data-ttu-id="5d3be-205">hello csak a hello normál rendszer URL-cím toohello alkalmazott küldi.</span><span class="sxs-lookup"><span data-stu-id="5d3be-205">hello system sends only hello normal system URL toohello employee.</span></span> <span data-ttu-id="5d3be-206">hello alkalmazott nem jelentkezik be egyszeri Bejelentkezést, és kell jelszó használata vagy a tanúsítvány helyette.</span><span class="sxs-lookup"><span data-stu-id="5d3be-206">hello employee cannot log on using SSO, and must use password or certificate instead.</span></span>
   
    <span data-ttu-id="5d3be-207">**EGYSZERI BEJELENTKEZÉSI URL-CÍME**</span><span class="sxs-lookup"><span data-stu-id="5d3be-207">**SSO URL**</span></span> 
   
    <span data-ttu-id="5d3be-208">hello csak hello egyszeri bejelentkezési URL-cím toohello alkalmazott küldi.</span><span class="sxs-lookup"><span data-stu-id="5d3be-208">hello system sends only hello SSO URL toohello employee.</span></span> <span data-ttu-id="5d3be-209">hello alkalmazott Egyszeri bejelentkezéshez használhatja.</span><span class="sxs-lookup"><span data-stu-id="5d3be-209">hello employee can log on using SSO.</span></span> <span data-ttu-id="5d3be-210">Hitelesítési kérelem hello IdP van irányítva.</span><span class="sxs-lookup"><span data-stu-id="5d3be-210">Authentication request is redirected through hello IdP.</span></span>
   
    <span data-ttu-id="5d3be-211">**Automatikus kiválasztásához.**</span><span class="sxs-lookup"><span data-stu-id="5d3be-211">**Automatic Selection**</span></span>
   
    <span data-ttu-id="5d3be-212">Ha az SSO nem aktív, hello küldi hello normál rendszer URL-cím toohello alkalmazott.</span><span class="sxs-lookup"><span data-stu-id="5d3be-212">If SSO is not active, hello system sends hello normal system URL toohello employee.</span></span> <span data-ttu-id="5d3be-213">Ha egyszeri bejelentkezés aktív, a hello rendszer ellenőrzi, hogy hello alkalmazott jelszóval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="5d3be-213">If SSO is active, hello system checks whether hello employee has a password.</span></span> <span data-ttu-id="5d3be-214">A jelszó nem érhető el, ha mind SSO és URL-címe nem-SSO küldött toohello alkalmazott.</span><span class="sxs-lookup"><span data-stu-id="5d3be-214">If a password is available, both SSO URL and Non-SSO URL are sent toohello employee.</span></span> <span data-ttu-id="5d3be-215">Azonban ha hello alkalmazott nincs jelszava, csak hello egyszeri bejelentkezési URL-cím küldött toohello alkalmazott.</span><span class="sxs-lookup"><span data-stu-id="5d3be-215">However, if hello employee has no password, only hello SSO URL is sent toohello employee.</span></span>
   
    <span data-ttu-id="5d3be-216">k.</span><span class="sxs-lookup"><span data-stu-id="5d3be-216">k.</span></span> <span data-ttu-id="5d3be-217">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="5d3be-217">Save your changes.</span></span>

> [!TIP]
> <span data-ttu-id="5d3be-218">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="5d3be-218">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5d3be-219">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="5d3be-219">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5d3be-220">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5d3be-220">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5d3be-221">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5d3be-221">Creating an Azure AD test user</span></span>
<span data-ttu-id="5d3be-222">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="5d3be-222">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="5d3be-224">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="5d3be-224">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5d3be-225">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5d3be-225">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5d3be-227">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="5d3be-227">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5d3be-229">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5d3be-229">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5d3be-231">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5d3be-231">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5d3be-233">a.</span><span class="sxs-lookup"><span data-stu-id="5d3be-233">a.</span></span> <span data-ttu-id="5d3be-234">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5d3be-234">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5d3be-235">b.</span><span class="sxs-lookup"><span data-stu-id="5d3be-235">b.</span></span> <span data-ttu-id="5d3be-236">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5d3be-236">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5d3be-237">c.</span><span class="sxs-lookup"><span data-stu-id="5d3be-237">c.</span></span> <span data-ttu-id="5d3be-238">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="5d3be-238">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5d3be-239">d.</span><span class="sxs-lookup"><span data-stu-id="5d3be-239">d.</span></span> <span data-ttu-id="5d3be-240">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5d3be-240">Click **Create**.</span></span>
 
### <a name="creating-a-sap-cloud-for-customer-test-user"></a><span data-ttu-id="5d3be-241">SAP felhő a felhasználói tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5d3be-241">Creating a SAP Cloud for Customer test user</span></span>

<span data-ttu-id="5d3be-242">Ebben a szakaszban egy SAP felhő Britta Simon nevű ügyfél felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5d3be-242">In this section, you create a user called Britta Simon in SAP Cloud for Customer.</span></span> <span data-ttu-id="5d3be-243">Adjon együttműködve [SAP felhő a felhasználói támogatási csoport](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) tooadd hello felhasználók hello SAP felhő ügyfél platform.</span><span class="sxs-lookup"><span data-stu-id="5d3be-243">Please work with [SAP Cloud for Customer support team](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) tooadd hello users in hello SAP Cloud for Customer platform.</span></span> 

> [!NOTE]
> <span data-ttu-id="5d3be-244">Győződjön meg arról, hogy NameID érték az ügyfél platform SAP felhő hello hello felhasználónév mezője egyezniük kell.</span><span class="sxs-lookup"><span data-stu-id="5d3be-244">Please make sure that NameID value should match with hello username field in hello SAP Cloud for Customer platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5d3be-245">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="5d3be-245">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5d3be-246">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezést a felhasználói hozzáférés tooSAP felhő megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="5d3be-246">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSAP Cloud for Customer.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="5d3be-248">**tooassign Britta Simon tooSAP felhő ügyfél, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="5d3be-248">**tooassign Britta Simon tooSAP Cloud for Customer, perform hello following steps:**</span></span>

1. <span data-ttu-id="5d3be-249">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5d3be-249">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="5d3be-251">Hello alkalmazások listában válassza ki a **SAP felhő ügyfél**.</span><span class="sxs-lookup"><span data-stu-id="5d3be-251">In hello applications list, select **SAP Cloud for Customer**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_app.png) 

3. <span data-ttu-id="5d3be-253">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="5d3be-253">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="5d3be-255">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="5d3be-255">Click **Add** button.</span></span> <span data-ttu-id="5d3be-256">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5d3be-256">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="5d3be-258">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="5d3be-258">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5d3be-259">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5d3be-259">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5d3be-260">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5d3be-260">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5d3be-261">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="5d3be-261">Testing single sign-on</span></span>

<span data-ttu-id="5d3be-262">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="5d3be-262">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5d3be-263">Hello SAP felhő a felhasználói csempe a hozzáférési Panel hello kattintáskor felhasználói alkalmazás kapja meg automatikusan bejelentkezett tooyour SAP felhő.</span><span class="sxs-lookup"><span data-stu-id="5d3be-263">When you click hello SAP Cloud for Customer tile in hello Access Panel, you should get automatically signed-on tooyour SAP Cloud for Customer application.</span></span>
<span data-ttu-id="5d3be-264">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5d3be-264">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5d3be-265">További források</span><span class="sxs-lookup"><span data-stu-id="5d3be-265">Additional resources</span></span>

* [<span data-ttu-id="5d3be-266">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="5d3be-266">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5d3be-267">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="5d3be-267">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_203.png

