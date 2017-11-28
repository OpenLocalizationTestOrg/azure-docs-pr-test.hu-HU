---
title: "aaaPrerequisites tooaccess hello Azure AD jelentéskészítési API |} Microsoft Docs"
description: "További tudnivalók: hello Előfeltételek tooaccess hello Azure AD jelentéskészítési API"
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
ms.openlocfilehash: ec28a7530f341dda31268a978754b615c727d66f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prerequisites-tooaccess-hello-azure-ad-reporting-api"></a><span data-ttu-id="64f9a-103">Előfeltételek tooaccess hello Azure AD jelentéskészítési API</span><span class="sxs-lookup"><span data-stu-id="64f9a-103">Prerequisites tooaccess hello Azure AD reporting API</span></span>

<span data-ttu-id="64f9a-104">Hello [az Azure AD reporting API-k](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) REST-alapú API-készlet toohello adatokat programozott hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="64f9a-104">hello [Azure AD reporting APIs](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) provide you with programmatic access toohello data through a set of REST-based APIs.</span></span> <span data-ttu-id="64f9a-105">Különböző programnyelvekkel és eszközökkel hívhatja ezeket az API-kat.</span><span class="sxs-lookup"><span data-stu-id="64f9a-105">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="64f9a-106">a reporting API által használt hello [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooauthorize hozzáférés toohello webes API-k.</span><span class="sxs-lookup"><span data-stu-id="64f9a-106">hello reporting API uses [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooauthorize access toohello web APIs.</span></span> 

<span data-ttu-id="64f9a-107">tooget adatelérés toohello jelentéskészítési hello API-n keresztül, szükséges toohave hello szerepkörök hozzárendelése a következő egyikét:</span><span class="sxs-lookup"><span data-stu-id="64f9a-107">tooget access toohello reporting data through hello API, you need toohave one of hello following roles assigned:</span></span>

- <span data-ttu-id="64f9a-108">Biztonsági olvasó</span><span class="sxs-lookup"><span data-stu-id="64f9a-108">Security Reader</span></span>
- <span data-ttu-id="64f9a-109">Biztonsági rendszergazda</span><span class="sxs-lookup"><span data-stu-id="64f9a-109">Security Admin</span></span>
- <span data-ttu-id="64f9a-110">Globális rendszergazda</span><span class="sxs-lookup"><span data-stu-id="64f9a-110">Global Admin</span></span>


<span data-ttu-id="64f9a-111">tooprepare a reporting API access toohello, kell:</span><span class="sxs-lookup"><span data-stu-id="64f9a-111">tooprepare your access toohello reporting API, you must:</span></span>

1. <span data-ttu-id="64f9a-112">Egy alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="64f9a-112">Register an application</span></span> 
2. <span data-ttu-id="64f9a-113">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="64f9a-113">Grant permissions</span></span> 
3. <span data-ttu-id="64f9a-114">Konfigurációs beállítások összegyűjtése</span><span class="sxs-lookup"><span data-stu-id="64f9a-114">Gather configuration settings</span></span> 

<span data-ttu-id="64f9a-115">Kérdéseit, vagy visszajelzést, kérjük, [fájlt egy támogatási jegy](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto).</span><span class="sxs-lookup"><span data-stu-id="64f9a-115">For questions, issues or feedback, please [file a support ticket](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto).</span></span>

## <a name="register-an-azure-active-directory-application"></a><span data-ttu-id="64f9a-116">Egy Azure Active Directory-alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="64f9a-116">Register an Azure Active Directory application</span></span>

<span data-ttu-id="64f9a-117">Akkor is, ha a reporting API-val parancsprogrammal hello elérésére kell tooregister egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="64f9a-117">You need tooregister an app even if you are accessing hello reporting API using a script.</span></span> <span data-ttu-id="64f9a-118">Ez lehetővé teszi egy **Alkalmazásazonosító**, ami azonban szükséges engedélyezési-hívások, és lehetővé teszi, hogy a kód tooreceive jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="64f9a-118">This gives you an **Application ID**, which is required for an authorization call and it enables your code tooreceive tokens.</span></span>

<span data-ttu-id="64f9a-119">tooconfigure a directory tooaccess hello Azure AD jelentéskészítési API, be kell jelentkeznie a toohello Azure-portálon az Azure rendszergazdai fiókkal, amely tagja is hello **globális rendszergazda** az Azure AD-bérlő a címtár szerepkörrel .</span><span class="sxs-lookup"><span data-stu-id="64f9a-119">tooconfigure your directory tooaccess hello Azure AD reporting API, you must sign in toohello Azure portal with an Azure administrator account that is also a member of hello **Global Administrator** directory role in your Azure AD tenant.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="64f9a-120">A "rendszergazda" jogosultságokkal credentials futó alkalmazások nagyon hatékonyak lehetnek, így Felhívjuk biztonságos meg arról, hogy tookeep hello alkalmazás azonosítója/titkos hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="64f9a-120">Applications running under credentials with "admin" privileges like this can be very powerful, so please be sure tookeep hello application's ID/secret credentials secure.</span></span>
> 


<span data-ttu-id="64f9a-121">**egy Azure Active Directory-alkalmazás tooregister:**</span><span class="sxs-lookup"><span data-stu-id="64f9a-121">**tooregister an Azure Active Directory application:**</span></span>

1. <span data-ttu-id="64f9a-122">A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-122">In hello [Azure portal](https://portal.azure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="64f9a-124">A hello **Azure Active Directory** panelen kattintson a **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-124">On hello **Azure Active Directory** blade, click **App registrations**.</span></span>

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/02.png) 

3. <span data-ttu-id="64f9a-126">A hello **App regisztrációk** panelen hello legfelül hello eszköztáron kattintson **új alkalmazás regisztrációja**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-126">On hello **App registrations** blade, in hello toolbar on hello top, click **New application registration**.</span></span>

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/03.png)

4. <span data-ttu-id="64f9a-128">A hello **létrehozása** panelen, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="64f9a-128">On hello **Create** blade, perform hello following steps:</span></span>

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/04.png)

    <span data-ttu-id="64f9a-130">a.</span><span class="sxs-lookup"><span data-stu-id="64f9a-130">a.</span></span> <span data-ttu-id="64f9a-131">A hello **neve** szövegmezőhöz típus `Reporting API application`.</span><span class="sxs-lookup"><span data-stu-id="64f9a-131">In hello **Name** textbox, type `Reporting API application`.</span></span>

    <span data-ttu-id="64f9a-132">b.</span><span class="sxs-lookup"><span data-stu-id="64f9a-132">b.</span></span> <span data-ttu-id="64f9a-133">Mint **alkalmazástípus**, jelölje be **Web app / API**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-133">As **Application type**, select **Web app / API**.</span></span>

    <span data-ttu-id="64f9a-134">c.</span><span class="sxs-lookup"><span data-stu-id="64f9a-134">c.</span></span> <span data-ttu-id="64f9a-135">A hello **bejelentkezési URL-cím** szövegmezőhöz típus `https://localhost`.</span><span class="sxs-lookup"><span data-stu-id="64f9a-135">In hello **Sign-on URL** textbox, type `https://localhost`.</span></span>

    <span data-ttu-id="64f9a-136">d.</span><span class="sxs-lookup"><span data-stu-id="64f9a-136">d.</span></span> <span data-ttu-id="64f9a-137">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="64f9a-137">Click **Create**.</span></span> 


## <a name="grant-permissions"></a><span data-ttu-id="64f9a-138">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="64f9a-138">Grant permissions</span></span> 

<span data-ttu-id="64f9a-139">Ez a lépés hello célját toogrant van az alkalmazás **címtáradatok olvasása** engedélyek toohello **Windows Azure Active Directory** API.</span><span class="sxs-lookup"><span data-stu-id="64f9a-139">hello objective of this step is toogrant your application **Read directory data** permissions toohello **Windows Azure Active Directory** API.</span></span>

![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/16.png)
 

<span data-ttu-id="64f9a-141">**toogrant az alkalmazás engedélyt toouse hello API:**</span><span class="sxs-lookup"><span data-stu-id="64f9a-141">**toogrant your application permission toouse hello API:**</span></span>

1. <span data-ttu-id="64f9a-142">A hello **App regisztrációk** panelen hello alkalmazások listájában, kattintson a **Reporting API-alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-142">On hello **App registrations** blade, in hello apps list, click **Reporting API application**.</span></span>

2. <span data-ttu-id="64f9a-143">A hello **Reporting API-alkalmazás** panelen hello legfelül hello eszköztáron kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-143">On hello **Reporting API application** blade, in hello toolbar on hello top, click **Settings**.</span></span> 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

3. <span data-ttu-id="64f9a-145">A hello **beállítások** panelen kattintson a **szükséges engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-145">On hello **Settings** blade, click **Required permissions**.</span></span> 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/06.png)

4. <span data-ttu-id="64f9a-147">A hello **szükséges engedélyek** paneljén, hello **API** listában, kattintson **Windows Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-147">On hello **Required permissions** blade, in hello **API** list, click **Windows Azure Active Directory**.</span></span> 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/07.png)

5. <span data-ttu-id="64f9a-149">A hello **hozzáférés engedélyezése** panelen válassza **címtáradatok olvasása**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-149">On hello **Enable Access** blade, select **Read directory data**.</span></span> 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/08.png)

6. <span data-ttu-id="64f9a-151">Hello hello felső eszköztárán kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-151">In hello toolbar on hello top, click **Save**.</span></span>

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/15.png)

## <a name="gather-configuration-settings"></a><span data-ttu-id="64f9a-153">Konfigurációs beállítások összegyűjtése</span><span class="sxs-lookup"><span data-stu-id="64f9a-153">Gather configuration settings</span></span> 
<span data-ttu-id="64f9a-154">Ez a szakasz bemutatja, hogy a címtárban lévő beállítások következő tooget hello:</span><span class="sxs-lookup"><span data-stu-id="64f9a-154">This section shows you how tooget hello following settings from your directory:</span></span>

* <span data-ttu-id="64f9a-155">Tartománynév</span><span class="sxs-lookup"><span data-stu-id="64f9a-155">Domain name</span></span>
* <span data-ttu-id="64f9a-156">Ügyfél-azonosító</span><span class="sxs-lookup"><span data-stu-id="64f9a-156">Client ID</span></span>
* <span data-ttu-id="64f9a-157">Titkos ügyfélkulcs</span><span class="sxs-lookup"><span data-stu-id="64f9a-157">Client secret</span></span>

<span data-ttu-id="64f9a-158">Hívások toohello jelentéskészítési API konfigurálásakor kell ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="64f9a-158">You need these values when configuring calls toohello reporting API.</span></span> 

### <a name="get-your-domain-name"></a><span data-ttu-id="64f9a-159">A tartománynév beolvasása</span><span class="sxs-lookup"><span data-stu-id="64f9a-159">Get your domain name</span></span>

<span data-ttu-id="64f9a-160">**tooget a tartománynév:**</span><span class="sxs-lookup"><span data-stu-id="64f9a-160">**tooget your domain name:**</span></span>

1. <span data-ttu-id="64f9a-161">A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-161">In hello [Azure portal](https://portal.azure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="64f9a-163">A hello **Azure Active Directory** panelen kattintson a **tartománynevek**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-163">On hello **Azure Active Directory** blade, click **Domain names**.</span></span>

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/09.png) 

3. <span data-ttu-id="64f9a-165">A tartománynév másolása tartományok hello listáját.</span><span class="sxs-lookup"><span data-stu-id="64f9a-165">Copy your domain name from hello list of domains.</span></span>


### <a name="get-your-applications-client-id"></a><span data-ttu-id="64f9a-166">Az alkalmazás ügyfél-azonosító lekérése</span><span class="sxs-lookup"><span data-stu-id="64f9a-166">Get your application's client ID</span></span>

<span data-ttu-id="64f9a-167">**tooget az alkalmazás ügyfél-azonosító:**</span><span class="sxs-lookup"><span data-stu-id="64f9a-167">**tooget your application's client ID:**</span></span>

1. <span data-ttu-id="64f9a-168">A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-168">In hello [Azure portal](https://portal.azure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="64f9a-170">A hello **App regisztrációk** panelen hello alkalmazások listájában, kattintson a **Reporting API-alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-170">On hello **App registrations** blade, in hello apps list, click **Reporting API application**.</span></span>

3. <span data-ttu-id="64f9a-171">A hello **Reporting API-alkalmazás** penge, hello **Alkalmazásazonosító**, kattintson a **toocopy kattintson**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-171">On hello **Reporting API application** blade, at hello **Application ID**, click **Click toocopy**.</span></span>

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/11.png) 



### <a name="get-your-applications-client-secret"></a><span data-ttu-id="64f9a-173">Az alkalmazás ügyfél titkos kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="64f9a-173">Get your application's client secret</span></span>
<span data-ttu-id="64f9a-174">tooget az alkalmazás ügyfél titkos, meg kell toocreate egy új kulcsot, és mentse után hello új kulcs mentése, mert már nem lehetséges tooretrieve értéke ezt az értéket később már.</span><span class="sxs-lookup"><span data-stu-id="64f9a-174">tooget your application's client secret, you need toocreate a new key and save its value upon saving hello new key because it is not possible tooretrieve this value later anymore.</span></span>

<span data-ttu-id="64f9a-175">**tooget az alkalmazás ügyfélkulcs:**</span><span class="sxs-lookup"><span data-stu-id="64f9a-175">**tooget your application's client secret:**</span></span>

1. <span data-ttu-id="64f9a-176">A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-176">In hello [Azure portal](https://portal.azure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="64f9a-178">A hello **App regisztrációk** panelen hello alkalmazások listájában, kattintson a **Reporting API-alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-178">On hello **App registrations** blade, in hello apps list, click **Reporting API application**.</span></span>


3. <span data-ttu-id="64f9a-179">A hello **Reporting API-alkalmazás** panelen hello legfelül hello eszköztáron kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-179">On hello **Reporting API application** blade, in hello toolbar on hello top, click **Settings**.</span></span> 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

4. <span data-ttu-id="64f9a-181">A hello **beállítások** paneljén, hello **APIR hozzáférés** területén kattintson **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-181">On hello **Settings** blade, in hello **APIR Access** section, click **Keys**.</span></span> 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/12.png)


5. <span data-ttu-id="64f9a-183">A hello **kulcsok** panelen, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="64f9a-183">On hello **Keys** blade, perform hello following steps:</span></span>

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/14.png)

    <span data-ttu-id="64f9a-185">a.</span><span class="sxs-lookup"><span data-stu-id="64f9a-185">a.</span></span> <span data-ttu-id="64f9a-186">A hello **leírás** szövegmezőhöz típus `Reporting API`.</span><span class="sxs-lookup"><span data-stu-id="64f9a-186">In hello **Description** textbox, type `Reporting API`.</span></span>

    <span data-ttu-id="64f9a-187">b.</span><span class="sxs-lookup"><span data-stu-id="64f9a-187">b.</span></span> <span data-ttu-id="64f9a-188">Mint **Expires**, jelölje be **2 évben**.</span><span class="sxs-lookup"><span data-stu-id="64f9a-188">As **Expires**, select **In 2 years**.</span></span>

    <span data-ttu-id="64f9a-189">c.</span><span class="sxs-lookup"><span data-stu-id="64f9a-189">c.</span></span> <span data-ttu-id="64f9a-190">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="64f9a-190">Click **Save**.</span></span>

    <span data-ttu-id="64f9a-191">d.</span><span class="sxs-lookup"><span data-stu-id="64f9a-191">d.</span></span> <span data-ttu-id="64f9a-192">Másolja a hello kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="64f9a-192">Copy hello key value.</span></span>


## <a name="next-steps"></a><span data-ttu-id="64f9a-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="64f9a-193">Next Steps</span></span>
* <span data-ttu-id="64f9a-194">Reporting volna, például tooaccess hello hello Azure AD adatait API-val programozott módon?</span><span class="sxs-lookup"><span data-stu-id="64f9a-194">Would you like tooaccess hello data from hello Azure AD reporting API in a programmatic manner?</span></span> <span data-ttu-id="64f9a-195">Tekintse meg [Ismerkedés az Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="64f9a-195">Check out [Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>
* <span data-ttu-id="64f9a-196">Ha szeretne további információk az Azure Active Directory reporting toofind, tekintse meg a hello [Azure Active Directory-jelentéskészítés – útmutató](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="64f9a-196">If you would like toofind out more about Azure Active Directory reporting, see hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span></span>  

