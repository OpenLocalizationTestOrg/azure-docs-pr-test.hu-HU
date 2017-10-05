---
title: "Előfeltételek az Azure AD reporting API eléréséhez |} Microsoft Docs"
description: "További tudnivalók az Azure AD reporting API eléréséhez Előfeltételek"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 5fafd83c337e3c73260d89cdad7409a01ce5855b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a><span data-ttu-id="d1aa0-103">Az Azure AD reporting API eléréséhez Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d1aa0-103">Prerequisites to access the Azure AD reporting API</span></span>

<span data-ttu-id="d1aa0-104">A [az Azure AD reporting API-k](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) keresztül REST-alapú API-készlet programozott hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-104">The [Azure AD reporting APIs](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) provide you with programmatic access to the data through a set of REST-based APIs.</span></span> <span data-ttu-id="d1aa0-105">Különböző programnyelvekkel és eszközökkel hívhatja ezeket az API-kat.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-105">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="d1aa0-106">A jelentéskészítési API által használt [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) engedélyezéséhez a web API-k eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-106">The reporting API uses [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) to authorize access to the web APIs.</span></span> 

<span data-ttu-id="d1aa0-107">A jelentési adatokat juthatnak az API-n keresztül, egy a következő szerepkörrel kell:</span><span class="sxs-lookup"><span data-stu-id="d1aa0-107">To get access to the reporting data through the API, you need to have one of the following roles assigned:</span></span>

- <span data-ttu-id="d1aa0-108">Biztonsági olvasó</span><span class="sxs-lookup"><span data-stu-id="d1aa0-108">Security Reader</span></span>
- <span data-ttu-id="d1aa0-109">Biztonsági rendszergazda</span><span class="sxs-lookup"><span data-stu-id="d1aa0-109">Security Admin</span></span>
- <span data-ttu-id="d1aa0-110">Globális rendszergazda</span><span class="sxs-lookup"><span data-stu-id="d1aa0-110">Global Admin</span></span>


<span data-ttu-id="d1aa0-111">Készítse elő a reporting API a hozzáférést, a következőket kell tennie:</span><span class="sxs-lookup"><span data-stu-id="d1aa0-111">To prepare your access to the reporting API, you must:</span></span>

1. <span data-ttu-id="d1aa0-112">Egy alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="d1aa0-112">Register an application</span></span> 
2. <span data-ttu-id="d1aa0-113">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="d1aa0-113">Grant permissions</span></span> 
3. <span data-ttu-id="d1aa0-114">Konfigurációs beállítások összegyűjtése</span><span class="sxs-lookup"><span data-stu-id="d1aa0-114">Gather configuration settings</span></span> 

<span data-ttu-id="d1aa0-115">Kérdéseit, vagy visszajelzést, kérjük, [fájlt egy támogatási jegy](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto).</span><span class="sxs-lookup"><span data-stu-id="d1aa0-115">For questions, issues or feedback, please [file a support ticket](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto).</span></span>

## <a name="register-an-azure-active-directory-application"></a><span data-ttu-id="d1aa0-116">Egy Azure Active Directory-alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="d1aa0-116">Register an Azure Active Directory application</span></span>

<span data-ttu-id="d1aa0-117">Kell regisztrálnia az alkalmazást, akkor is, ha a jelentéskészítési API parancsfájl segítségével fér hozzá.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-117">You need to register an app even if you are accessing the reporting API using a script.</span></span> <span data-ttu-id="d1aa0-118">Ez lehetővé teszi egy **Alkalmazásazonosító**, ami azonban szükséges engedélyezési-hívások, és lehetővé teszi, hogy a kód jogkivonatokat fogadni.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-118">This gives you an **Application ID**, which is required for an authorization call and it enables your code to receive tokens.</span></span>

<span data-ttu-id="d1aa0-119">A címtár az Azure AD reporting API eléréséhez konfigurálásához be kell jelentkeznie az Azure portálra az Azure rendszergazdai fiókkal, amely tagja a **globális rendszergazda** az Azure AD-bérlő a címtár szerepkörrel.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-119">To configure your directory to access the Azure AD reporting API, you must sign in to the Azure portal with an Azure administrator account that is also a member of the **Global Administrator** directory role in your Azure AD tenant.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d1aa0-120">Ehhez hasonló "rendszergazda" jogosultságokkal credentials futó alkalmazások nagyon hatékonyak lehetnek, így az alkalmazás azonosítója/titkos hitelesítő adatok a biztonságos ellenőrizze.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-120">Applications running under credentials with "admin" privileges like this can be very powerful, so please be sure to keep the application's ID/secret credentials secure.</span></span>
> 


<span data-ttu-id="d1aa0-121">**Egy Azure Active Directory-alkalmazás regisztrálása:**</span><span class="sxs-lookup"><span data-stu-id="d1aa0-121">**To register an Azure Active Directory application:**</span></span>

1. <span data-ttu-id="d1aa0-122">Az a [Azure-portálon](https://portal.azure.com), a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-122">In the [Azure portal](https://portal.azure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="d1aa0-124">Az a **Azure Active Directory** panelen kattintson a **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-124">On the **Azure Active Directory** blade, click **App registrations**.</span></span>

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/02.png) 

3. <span data-ttu-id="d1aa0-126">Az a **App regisztrációk** panelen, a felső eszköztáron kattintson **új alkalmazás regisztrációja**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-126">On the **App registrations** blade, in the toolbar on the top, click **New application registration**.</span></span>

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/03.png)

4. <span data-ttu-id="d1aa0-128">Az a **létrehozása** panelen végezze el a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d1aa0-128">On the **Create** blade, perform the following steps:</span></span>

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/04.png)

    <span data-ttu-id="d1aa0-130">a.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-130">a.</span></span> <span data-ttu-id="d1aa0-131">Az a **neve** szövegmezőhöz típus `Reporting API application`.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-131">In the **Name** textbox, type `Reporting API application`.</span></span>

    <span data-ttu-id="d1aa0-132">b.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-132">b.</span></span> <span data-ttu-id="d1aa0-133">Mint **alkalmazástípus**, jelölje be **Web app / API**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-133">As **Application type**, select **Web app / API**.</span></span>

    <span data-ttu-id="d1aa0-134">c.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-134">c.</span></span> <span data-ttu-id="d1aa0-135">Az a **bejelentkezési URL-cím** szövegmezőhöz típus `https://localhost`.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-135">In the **Sign-on URL** textbox, type `https://localhost`.</span></span>

    <span data-ttu-id="d1aa0-136">d.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-136">d.</span></span> <span data-ttu-id="d1aa0-137">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-137">Click **Create**.</span></span> 


## <a name="grant-permissions"></a><span data-ttu-id="d1aa0-138">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="d1aa0-138">Grant permissions</span></span> 

<span data-ttu-id="d1aa0-139">E lépés célja, hogy adja meg az alkalmazás **címtáradatok olvasása** engedélyeket a **Windows Azure Active Directory** API.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-139">The objective of this step is to grant your application **Read directory data** permissions to the **Windows Azure Active Directory** API.</span></span>

![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/16.png)
 

<span data-ttu-id="d1aa0-141">**Az alkalmazás engedélyezheti az API-t használja:**</span><span class="sxs-lookup"><span data-stu-id="d1aa0-141">**To grant your application permission to use the API:**</span></span>

1. <span data-ttu-id="d1aa0-142">Az a **App regisztrációk** panel, az alkalmazások listájában kattintson **Reporting API-alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-142">On the **App registrations** blade, in the apps list, click **Reporting API application**.</span></span>

2. <span data-ttu-id="d1aa0-143">Az a **Reporting API-alkalmazás** panelen, a felső eszköztáron kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-143">On the **Reporting API application** blade, in the toolbar on the top, click **Settings**.</span></span> 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

3. <span data-ttu-id="d1aa0-145">Az a **beállítások** panelen kattintson a **szükséges engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-145">On the **Settings** blade, click **Required permissions**.</span></span> 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/06.png)

4. <span data-ttu-id="d1aa0-147">Az a **szükséges engedélyek** panelen, a a **API** listában, kattintson **Windows Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-147">On the **Required permissions** blade, in the **API** list, click **Windows Azure Active Directory**.</span></span> 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/07.png)

5. <span data-ttu-id="d1aa0-149">Az a **hozzáférés engedélyezése** panelen válassza **címtáradatok olvasása**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-149">On the **Enable Access** blade, select **Read directory data**.</span></span> 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/08.png)

6. <span data-ttu-id="d1aa0-151">A felső eszköztáron kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-151">In the toolbar on the top, click **Save**.</span></span>

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/15.png)

## <a name="gather-configuration-settings"></a><span data-ttu-id="d1aa0-153">Konfigurációs beállítások összegyűjtése</span><span class="sxs-lookup"><span data-stu-id="d1aa0-153">Gather configuration settings</span></span> 
<span data-ttu-id="d1aa0-154">Ez a szakasz bemutatja, hogyan az a Directoryból olvassa ki az alábbi beállításokat:</span><span class="sxs-lookup"><span data-stu-id="d1aa0-154">This section shows you how to get the following settings from your directory:</span></span>

* <span data-ttu-id="d1aa0-155">Tartománynév</span><span class="sxs-lookup"><span data-stu-id="d1aa0-155">Domain name</span></span>
* <span data-ttu-id="d1aa0-156">Ügyfél-azonosító</span><span class="sxs-lookup"><span data-stu-id="d1aa0-156">Client ID</span></span>
* <span data-ttu-id="d1aa0-157">Titkos ügyfélkulcs</span><span class="sxs-lookup"><span data-stu-id="d1aa0-157">Client secret</span></span>

<span data-ttu-id="d1aa0-158">A jelentéskészítési API hívásainak konfigurálásakor kell ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-158">You need these values when configuring calls to the reporting API.</span></span> 

### <a name="get-your-domain-name"></a><span data-ttu-id="d1aa0-159">A tartománynév beolvasása</span><span class="sxs-lookup"><span data-stu-id="d1aa0-159">Get your domain name</span></span>

<span data-ttu-id="d1aa0-160">**A tartománynév beolvasása:**</span><span class="sxs-lookup"><span data-stu-id="d1aa0-160">**To get your domain name:**</span></span>

1. <span data-ttu-id="d1aa0-161">Az a [Azure-portálon](https://portal.azure.com), a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-161">In the [Azure portal](https://portal.azure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="d1aa0-163">Az a **Azure Active Directory** panelen kattintson a **tartománynevek**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-163">On the **Azure Active Directory** blade, click **Domain names**.</span></span>

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/09.png) 

3. <span data-ttu-id="d1aa0-165">Másolja ki a tartomány nevét a tartományok listáját.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-165">Copy your domain name from the list of domains.</span></span>


### <a name="get-your-applications-client-id"></a><span data-ttu-id="d1aa0-166">Az alkalmazás ügyfél-azonosító lekérése</span><span class="sxs-lookup"><span data-stu-id="d1aa0-166">Get your application's client ID</span></span>

<span data-ttu-id="d1aa0-167">**Az alkalmazás ügyfél-azonosító eléréséhez:**</span><span class="sxs-lookup"><span data-stu-id="d1aa0-167">**To get your application's client ID:**</span></span>

1. <span data-ttu-id="d1aa0-168">Az a [Azure-portálon](https://portal.azure.com), a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-168">In the [Azure portal](https://portal.azure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="d1aa0-170">Az a **App regisztrációk** panel, az alkalmazások listájában kattintson **Reporting API-alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-170">On the **App registrations** blade, in the apps list, click **Reporting API application**.</span></span>

3. <span data-ttu-id="d1aa0-171">Az a **Reporting API-alkalmazás** panelen, a **Alkalmazásazonosító**, kattintson a **másolásához kattintson a**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-171">On the **Reporting API application** blade, at the **Application ID**, click **Click to copy**.</span></span>

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/11.png) 



### <a name="get-your-applications-client-secret"></a><span data-ttu-id="d1aa0-173">Az alkalmazás ügyfél titkos kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="d1aa0-173">Get your application's client secret</span></span>
<span data-ttu-id="d1aa0-174">Ahhoz, hogy az alkalmazás ügyfélkulcs, meg kell hozzon létre egy új kulcsot, és mentse az új kulcs mentése, mert nincs lehetőség a később többé lekérje ezt az értéket, az értékét.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-174">To get your application's client secret, you need to create a new key and save its value upon saving the new key because it is not possible to retrieve this value later anymore.</span></span>

<span data-ttu-id="d1aa0-175">**Az alkalmazás ügyfélkulcs beolvasása:**</span><span class="sxs-lookup"><span data-stu-id="d1aa0-175">**To get your application's client secret:**</span></span>

1. <span data-ttu-id="d1aa0-176">Az a [Azure-portálon](https://portal.azure.com), a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-176">In the [Azure portal](https://portal.azure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="d1aa0-178">Az a **App regisztrációk** panel, az alkalmazások listájában kattintson **Reporting API-alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-178">On the **App registrations** blade, in the apps list, click **Reporting API application**.</span></span>


3. <span data-ttu-id="d1aa0-179">Az a **Reporting API-alkalmazás** panelen, a felső eszköztáron kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-179">On the **Reporting API application** blade, in the toolbar on the top, click **Settings**.</span></span> 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

4. <span data-ttu-id="d1aa0-181">A a **beállítások** panelen, a a **APIR hozzáférés** kattintson **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-181">On the **Settings** blade, in the **APIR Access** section, click **Keys**.</span></span> 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/12.png)


5. <span data-ttu-id="d1aa0-183">Az a **kulcsok** panelen végezze el a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d1aa0-183">On the **Keys** blade, perform the following steps:</span></span>

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/14.png)

    <span data-ttu-id="d1aa0-185">a.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-185">a.</span></span> <span data-ttu-id="d1aa0-186">Az a **leírás** szövegmezőhöz típus `Reporting API`.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-186">In the **Description** textbox, type `Reporting API`.</span></span>

    <span data-ttu-id="d1aa0-187">b.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-187">b.</span></span> <span data-ttu-id="d1aa0-188">Mint **Expires**, jelölje be **2 évben**.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-188">As **Expires**, select **In 2 years**.</span></span>

    <span data-ttu-id="d1aa0-189">c.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-189">c.</span></span> <span data-ttu-id="d1aa0-190">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-190">Click **Save**.</span></span>

    <span data-ttu-id="d1aa0-191">d.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-191">d.</span></span> <span data-ttu-id="d1aa0-192">Másolja a kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="d1aa0-192">Copy the key value.</span></span>


## <a name="next-steps"></a><span data-ttu-id="d1aa0-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d1aa0-193">Next Steps</span></span>
* <span data-ttu-id="d1aa0-194">Szeretné az Azure AD reporting API-val programozott módon érheti el az adatait?</span><span class="sxs-lookup"><span data-stu-id="d1aa0-194">Would you like to access the data from the Azure AD reporting API in a programmatic manner?</span></span> <span data-ttu-id="d1aa0-195">Tekintse meg [Bevezetés az Azure Active Directory Reporting API használatába](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="d1aa0-195">Check out [Getting started with the Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>
* <span data-ttu-id="d1aa0-196">Ha meg szeretne többet megtudni az Azure Active Directory reporting, tekintse meg a [Azure Active Directory-jelentéskészítés – útmutató](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="d1aa0-196">If you would like to find out more about Azure Active Directory reporting, see the [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span></span>  

