---
title: "Közzétett alkalmazások egyéni kezdőlapját beállítása az Azure AD-alkalmazásproxy használatával |} Microsoft Docs"
description: "Ismerteti az alapvető tudnivalók az Azure AD-alkalmazásproxy-összekötő"
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 9069166259265f5d2b43043b75039e239f397f6c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a><span data-ttu-id="926e2-103">Közzétett alkalmazások egyéni kezdőlapját beállítása az Azure AD-alkalmazásproxy használatával</span><span class="sxs-lookup"><span data-stu-id="926e2-103">Set a custom home page for published apps by using Azure AD Application Proxy</span></span>

<span data-ttu-id="926e2-104">A cikk ismerteti a felhasználók számára egy egyéni kezdőlap közvetlen alkalmazások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="926e2-104">This article discusses how to configure apps to direct users to a custom home page.</span></span> <span data-ttu-id="926e2-105">Amikor közzétesz egy alkalmazást az alkalmazásproxy, be kell állítani egy belső URL-Címeket de időnként nem a lapon, a felhasználók először kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="926e2-105">When you publish an application with Application Proxy, you set an internal URL but sometimes that's not the page your users should see first.</span></span> <span data-ttu-id="926e2-106">Egy egyéni kezdőlap beállításáról, hogy a felhasználók Ugrás a jobb oldalon, az alkalmazások az Azure Active Directory-hozzáférési panelre vagy az Office 365 alkalmazás indító elérésekor.</span><span class="sxs-lookup"><span data-stu-id="926e2-106">Set a custom home page so that your users go to the right page when they access the apps from the Azure Active Directory Access Panel or the Office 365 app launcher.</span></span>

<span data-ttu-id="926e2-107">Amikor a felhasználók indítsa el az alkalmazást, azokat a legfelső szintű tartomány URL-címet a közzétett alkalmazás alapértelmezés szerint van irányítva.</span><span class="sxs-lookup"><span data-stu-id="926e2-107">When users launch the app, they're directed by default to the root domain URL for the published app.</span></span> <span data-ttu-id="926e2-108">Általában be van állítva az kezdőlapja a kezdőlap URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="926e2-108">The landing page is typically set as the home page URL.</span></span> <span data-ttu-id="926e2-109">Az Azure AD PowerShell modul segítségével egyéni kezdőlap URL-címek megadása, ha azt szeretné, hogy a felhasználók számára az alkalmazáson belül egy adott lap meg.</span><span class="sxs-lookup"><span data-stu-id="926e2-109">Use the Azure AD PowerShell module to define custom home page URLs when you want app users to land on a specific page within the app.</span></span> 

<span data-ttu-id="926e2-110">Példa:</span><span class="sxs-lookup"><span data-stu-id="926e2-110">For example:</span></span>
- <span data-ttu-id="926e2-111">A vállalati hálózaton belüli felhasználók Ugrás *https://ExpenseApp/login/login.aspx* jelentkezzen be, és az alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="926e2-111">Inside your corporate network, users go to *https://ExpenseApp/login/login.aspx* to sign in and access your app.</span></span>
- <span data-ttu-id="926e2-112">Mivel más eszközök, például alkalmazásproxy a mappaszerkezet legfelső szintjén hozzáférésre van szüksége a lemezképeket, tesz közzé az alkalmazást *https://ExpenseApp* az belső URL-címet.</span><span class="sxs-lookup"><span data-stu-id="926e2-112">Because you have other assets like images that Application Proxy needs to access at the top level of the folder structure, you publish the app with *https://ExpenseApp* as the internal URL.</span></span>
- <span data-ttu-id="926e2-113">Az alapértelmezett külső URL-címe *https://ExpenseApp-contoso.msappproxy.net*, amely nem veszi a felhasználókat, hogy a bejelentkezési oldalon.</span><span class="sxs-lookup"><span data-stu-id="926e2-113">The default external URL is *https://ExpenseApp-contoso.msappproxy.net*, which doesn't take your users to the sign in page.</span></span>  
- <span data-ttu-id="926e2-114">Állítsa be *https://ExpenseApp-contoso.msappproxy.net/login/login.aspx* az kezdőlap URL-címet kell biztosítani a felhasználóknak zökkenőmentes élményt.</span><span class="sxs-lookup"><span data-stu-id="926e2-114">Set *https://ExpenseApp-contoso.msappproxy.net/login/login.aspx* as the home page URL to give your users a seamless experience.</span></span> 

>[!NOTE]
><span data-ttu-id="926e2-115">Amikor a felhasználók hozzáférést adhat közzétett alkalmazások, az alkalmazások jelennek meg a [Azure AD hozzáférési Panel](active-directory-saas-access-panel-introduction.md) és a [Office 365 alkalmazás indító](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).</span><span class="sxs-lookup"><span data-stu-id="926e2-115">When you give users access to published apps, the apps are displayed in the [Azure AD Access Panel](active-directory-saas-access-panel-introduction.md) and the [Office 365 app launcher](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="926e2-116">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="926e2-116">Before you start</span></span>

<span data-ttu-id="926e2-117">Mielőtt a kezdőlap URL-Címének beállításához vegye figyelembe az alábbi követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="926e2-117">Before you set the home page URL, keep in mind the following requirements:</span></span>

* <span data-ttu-id="926e2-118">Győződjön meg arról, hogy a megadott elérési út egy altartomány elérési útját a legfelső szintű tartomány URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="926e2-118">Ensure that the path you specify is a subdomain path of the root domain URL.</span></span>

  <span data-ttu-id="926e2-119">Ha a legfelső szintű tartományi URL-CÍMÉT, például https://apps.contoso.com/app1/, a kezdőlap URL-címet, konfigurálnia kell kezdődnie https://apps.contoso.com/app1/.</span><span class="sxs-lookup"><span data-stu-id="926e2-119">If the root-domain URL is, for example, https://apps.contoso.com/app1/, the home page URL that you configure must start with https://apps.contoso.com/app1/.</span></span>

* <span data-ttu-id="926e2-120">Ha módosítja a közzétett alkalmazáshoz, a módosítás lehet, hogy állítsa vissza a kezdőlap URL-címet.</span><span class="sxs-lookup"><span data-stu-id="926e2-120">If you make a change to the published app, the change might reset the value of the home page URL.</span></span> <span data-ttu-id="926e2-121">Ha a jövőben frissíti az alkalmazás, inkább újbóli ellenőrzéséhez és, ha szükséges, frissítse a kezdőlap URL-címe.</span><span class="sxs-lookup"><span data-stu-id="926e2-121">When you update the app in the future, you should recheck and, if necessary, update the home page URL.</span></span>

## <a name="change-the-home-page-in-the-azure-portal"></a><span data-ttu-id="926e2-122">Módosítsa a kezdőlap, az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="926e2-122">Change the home page in the Azure portal</span></span>

1. <span data-ttu-id="926e2-123">Jelentkezzen be az [Azure Portal](https://portal.azure.com) felületére rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="926e2-123">Sign in to the [Azure portal](https://portal.azure.com) as an administrator.</span></span>
2. <span data-ttu-id="926e2-124">Navigáljon a **Azure Active Directory** > **App regisztrációk** , és válassza ki az alkalmazást a listából.</span><span class="sxs-lookup"><span data-stu-id="926e2-124">Navigate to **Azure Active Directory** > **App registrations** and choose your application from the list.</span></span> 
3. <span data-ttu-id="926e2-125">Válassza ki **tulajdonságok** beállításai közül.</span><span class="sxs-lookup"><span data-stu-id="926e2-125">Select **Properties** from the settings.</span></span>
4. <span data-ttu-id="926e2-126">Frissítés a **Kezdőlap** mező az új elérési úttal.</span><span class="sxs-lookup"><span data-stu-id="926e2-126">Update the **Home page URL** field with your new path.</span></span> 

   ![Új kezdőlap URL-Címének megadása](./media/application-proxy-office365-app-launcher/homepage.png)

5. <span data-ttu-id="926e2-128">Válassza ki **mentése**</span><span class="sxs-lookup"><span data-stu-id="926e2-128">Select **Save**</span></span>

## <a name="change-the-home-page-with-powershell"></a><span data-ttu-id="926e2-129">Módosítsa a kezdőlap a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="926e2-129">Change the home page with PowerShell</span></span>

### <a name="install-the-azure-ad-powershell-module"></a><span data-ttu-id="926e2-130">Az Azure AD PowerShell modul telepítése</span><span class="sxs-lookup"><span data-stu-id="926e2-130">Install the Azure AD PowerShell module</span></span>

<span data-ttu-id="926e2-131">Mielőtt egy egyéni kezdőlap URL-Címének megadása PowerShell segítségével, telepítse az Azure AD PowerShell modult.</span><span class="sxs-lookup"><span data-stu-id="926e2-131">Before you define a custom home page URL by using PowerShell, install the Azure AD PowerShell module.</span></span> <span data-ttu-id="926e2-132">A csomagot a programot letöltheti a [PowerShell-galériában](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131), amely használja, a Graph API-végpont.</span><span class="sxs-lookup"><span data-stu-id="926e2-132">You can download the package from the [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131), which uses the Graph API endpoint.</span></span> 

<span data-ttu-id="926e2-133">A csomag telepítéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="926e2-133">To install the package, follow these steps:</span></span>

1. <span data-ttu-id="926e2-134">Nyissa meg a standard PowerShell-ablakot, és futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="926e2-134">Open a standard PowerShell window, and then run the following command:</span></span>

    ```
     Install-Module -Name AzureAD
    ```
    <span data-ttu-id="926e2-135">Ha a parancs fut, a nem rendszergazda, használja a `-scope currentuser` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="926e2-135">If you're running the command as a non-admin, use the `-scope currentuser` option.</span></span>
2. <span data-ttu-id="926e2-136">Válassza ki a telepítés során **Y** Nuget.org két csomagok telepítését. Mindkét csomagot szükség.</span><span class="sxs-lookup"><span data-stu-id="926e2-136">During the installation, select **Y** to install two packages from Nuget.org. Both packages are required.</span></span> 

### <a name="find-the-objectid-of-the-app"></a><span data-ttu-id="926e2-137">Az alkalmazás ObjectID keresése</span><span class="sxs-lookup"><span data-stu-id="926e2-137">Find the ObjectID of the app</span></span>

<span data-ttu-id="926e2-138">Szerezze be az alkalmazás ObjectID, majd keresse meg az alkalmazás által a kezdőlap.</span><span class="sxs-lookup"><span data-stu-id="926e2-138">Obtain the ObjectID of the app, and then search for the app by its home page.</span></span>

1. <span data-ttu-id="926e2-139">Nyissa meg a Powershellt, és importálja az Azure AD-modullal.</span><span class="sxs-lookup"><span data-stu-id="926e2-139">Open PowerShell and import the Azure AD module.</span></span>

    ```
    Import-Module AzureAD
    ```

2. <span data-ttu-id="926e2-140">Jelentkezzen be az Azure AD-modullal a bérlői rendszergazda felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="926e2-140">Sign in to the Azure AD module as the tenant administrator.</span></span>

    ```
    Connect-AzureAD
    ```
3. <span data-ttu-id="926e2-141">Megkeresheti az alkalmazást, a kezdőlap URL-címe alapján.</span><span class="sxs-lookup"><span data-stu-id="926e2-141">Find the app based on its home page URL.</span></span> <span data-ttu-id="926e2-142">Az URL-címet az található a portál a **Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="926e2-142">You can find the URL in the portal by going to **Azure Active Directory** > **Enterprise applications** > **All applications**.</span></span> <span data-ttu-id="926e2-143">Ez a példa *sharepoint-iddemo*.</span><span class="sxs-lookup"><span data-stu-id="926e2-143">This example uses *sharepoint-iddemo*.</span></span>

    ```
    Get-AzureADApplication | where { $_.Homepage -like “sharepoint-iddemo” } | fl DisplayName, Homepage, ObjectID
    ```
4. <span data-ttu-id="926e2-144">Az itt látható egy hasonló eredményt kapja meg.</span><span class="sxs-lookup"><span data-stu-id="926e2-144">You should get a result that's similar to the one shown here.</span></span> <span data-ttu-id="926e2-145">Másolja a ObjectID globálisan egyedi Azonosítót a következő szakaszban található.</span><span class="sxs-lookup"><span data-stu-id="926e2-145">Copy the ObjectID GUID to use in the next section.</span></span>

    ```
    DisplayName : SharePoint
    Homepage    : https://sharepoint-iddemo.msappproxy.net/
    ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

### <a name="update-the-home-page-url"></a><span data-ttu-id="926e2-146">A kezdőlap URL-Címének frissítése</span><span class="sxs-lookup"><span data-stu-id="926e2-146">Update the home page URL</span></span>

<span data-ttu-id="926e2-147">A ugyanazon PowerShell-modul az 1. lépésben használt hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="926e2-147">In the same PowerShell module that you used for step 1, perform the following steps:</span></span>

1. <span data-ttu-id="926e2-148">Győződjön meg arról, hogy rendelkezik a megfelelő alkalmazást, és cserélje le *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* rendelkező, az előző lépésben másolt objektumazonosító.</span><span class="sxs-lookup"><span data-stu-id="926e2-148">Confirm that you have the correct app, and replace *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* with the ObjectID that you copied in the preceding step.</span></span>

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4.
    ```

 <span data-ttu-id="926e2-149">Most, hogy az alkalmazás megerősítését készen áll a kezdőlap, az alábbiak szerint frissítse.</span><span class="sxs-lookup"><span data-stu-id="926e2-149">Now that you've confirmed the app, you're ready to update the home page, as follows.</span></span>

2. <span data-ttu-id="926e2-150">Hozzon létre egy üres alkalmazásobjektum ahhoz, hogy engedélyezni szeretné a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="926e2-150">Create a blank application object to hold the changes that you want to make.</span></span> <span data-ttu-id="926e2-151">Ez a változó a frissíteni kívánt értékeket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="926e2-151">This variable holds the values that you want to update.</span></span> <span data-ttu-id="926e2-152">Ezt a lépést nem jön létre.</span><span class="sxs-lookup"><span data-stu-id="926e2-152">Nothing is created in this step.</span></span>

    ```
    $appnew = New-Object “Microsoft.Open.AzureAD.Model.Application”
    ```

3. <span data-ttu-id="926e2-153">A kezdőlap URL-Címének beállítása a kívánt értékre.</span><span class="sxs-lookup"><span data-stu-id="926e2-153">Set the home page URL to the value that you want.</span></span> <span data-ttu-id="926e2-154">Az érték lehet egy altartomány elérési utat a közzétett alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="926e2-154">The value must be a subdomain path of the published app.</span></span> <span data-ttu-id="926e2-155">Például, ha módosítja a kezdőlap URL-CÍMÉT *https://sharepoint-iddemo.msappproxy.net/* való *https://sharepoint-iddemo.msappproxy.net/hybrid/*, az alkalmazásaik felhasználóit közvetlenül Ugrás az egyéni webhelyre.</span><span class="sxs-lookup"><span data-stu-id="926e2-155">For example, if you change the home page URL from *https://sharepoint-iddemo.msappproxy.net/* to *https://sharepoint-iddemo.msappproxy.net/hybrid/*, app users go directly to the custom home page.</span></span>

    ```
    $homepage = “https://sharepoint-iddemo.msappproxy.net/hybrid/”
    ```
4. <span data-ttu-id="926e2-156">A frissítéshez a GUID azonosítója (ObjectID) használatával "1. lépés: az alkalmazás ObjectID található."</span><span class="sxs-lookup"><span data-stu-id="926e2-156">Make the update by using the GUID (ObjectID) that you copied in "Step 1: Find the ObjectID of the app."</span></span>

    ```
    Set-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4 -Homepage $homepage
    ```
5. <span data-ttu-id="926e2-157">Győződjön meg arról, hogy sikeres volt-e a módosítást, indítsa újra az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="926e2-157">To confirm that the change was successful, restart the app.</span></span>

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

>[!NOTE]
><span data-ttu-id="926e2-158">Az alkalmazás végzett módosításokat előfordulhat, hogy alaphelyzetbe állítja a kezdőlap URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="926e2-158">Any changes that you make to the app might reset the home page URL.</span></span> <span data-ttu-id="926e2-159">Ha alaphelyzetbe állítja a kezdőlap URL-CÍMÉT, ismételje meg a 2.</span><span class="sxs-lookup"><span data-stu-id="926e2-159">If your home page URL resets, repeat step 2.</span></span>

## <a name="next-steps"></a><span data-ttu-id="926e2-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="926e2-160">Next steps</span></span>

- [<span data-ttu-id="926e2-161">Az Azure AD alkalmazásproxy SharePoint távoli hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="926e2-161">Enable remote access to SharePoint with Azure AD Application Proxy</span></span>](application-proxy-enable-remote-access-sharepoint.md)
- [<span data-ttu-id="926e2-162">Alkalmazásproxy engedélyezése az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="926e2-162">Enable Application Proxy in the Azure portal</span></span>](active-directory-application-proxy-enable.md)
