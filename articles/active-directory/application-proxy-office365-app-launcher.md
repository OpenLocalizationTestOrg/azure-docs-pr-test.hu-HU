---
title: "közzétett alkalmazások az Azure AD-alkalmazásproxy használatával egyéni kezdőlapját aaaSet |} Microsoft Docs"
description: "Magában foglalja az hello alapvető tudnivalók az Azure AD-alkalmazásproxy-összekötő"
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
ms.openlocfilehash: 5bb695e904d285c3b440520f107c7bf63ba5cac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a><span data-ttu-id="49ba7-103">Közzétett alkalmazások egyéni kezdőlapját beállítása az Azure AD-alkalmazásproxy használatával</span><span class="sxs-lookup"><span data-stu-id="49ba7-103">Set a custom home page for published apps by using Azure AD Application Proxy</span></span>

<span data-ttu-id="49ba7-104">Ez a cikk azt ismerteti, hogyan tooconfigure alkalmazások toodirect felhasználók tooa egyéni kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="49ba7-104">This article discusses how tooconfigure apps toodirect users tooa custom home page.</span></span> <span data-ttu-id="49ba7-105">Amikor közzétesz egy alkalmazást a Proxy, egy belső URL-Címének beállítása, de egyes esetekben a rendszer hello lapot a felhasználók először kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="49ba7-105">When you publish an application with Application Proxy, you set an internal URL but sometimes that's not hello page your users should see first.</span></span> <span data-ttu-id="49ba7-106">Egy egyéni kezdőlap beállításáról, hogy a felhasználók toohello jobb oldal nyissa meg a hello Azure Active Directory hozzáférési Panel vagy Office 365 hello alkalmazás indító hello alkalmazások elérésekor.</span><span class="sxs-lookup"><span data-stu-id="49ba7-106">Set a custom home page so that your users go toohello right page when they access hello apps from hello Azure Active Directory Access Panel or hello Office 365 app launcher.</span></span>

<span data-ttu-id="49ba7-107">Amikor felhasználók hello alkalmazást, akkor még útmutatása alapján alapértelmezett toohello legfelső szintű tartomány URL-hello közzétett alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="49ba7-107">When users launch hello app, they're directed by default toohello root domain URL for hello published app.</span></span> <span data-ttu-id="49ba7-108">általában be van állítva az hello kezdőlapja hello kezdőlap URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="49ba7-108">hello landing page is typically set as hello home page URL.</span></span> <span data-ttu-id="49ba7-109">Hello Azure AD PowerShell modul toodefine egyéni kezdőlap URL-címek használja, ha azt szeretné, hogy app felhasználók tooland hello alkalmazáson belül egy adott oldalon.</span><span class="sxs-lookup"><span data-stu-id="49ba7-109">Use hello Azure AD PowerShell module toodefine custom home page URLs when you want app users tooland on a specific page within hello app.</span></span> 

<span data-ttu-id="49ba7-110">Példa:</span><span class="sxs-lookup"><span data-stu-id="49ba7-110">For example:</span></span>
- <span data-ttu-id="49ba7-111">A vállalati hálózaton belüli felhasználók nyissa meg túl*https://ExpenseApp/login/login.aspx* a toosign és az alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="49ba7-111">Inside your corporate network, users go too*https://ExpenseApp/login/login.aspx* toosign in and access your app.</span></span>
- <span data-ttu-id="49ba7-112">Mivel más eszközök, például, hogy az alkalmazásproxy kell hello felső szintjén hello mappaszerkezet tooaccess képek, hello alkalmazás közzététele *https://ExpenseApp* hello belső URL-cím szerint.</span><span class="sxs-lookup"><span data-stu-id="49ba7-112">Because you have other assets like images that Application Proxy needs tooaccess at hello top level of hello folder structure, you publish hello app with *https://ExpenseApp* as hello internal URL.</span></span>
- <span data-ttu-id="49ba7-113">külső URL-cím alapértelmezett hello *https://ExpenseApp-contoso.msappproxy.net*, amely nem veszi a felhasználók toohello bejelentkezési oldalán.</span><span class="sxs-lookup"><span data-stu-id="49ba7-113">hello default external URL is *https://ExpenseApp-contoso.msappproxy.net*, which doesn't take your users toohello sign in page.</span></span>  
- <span data-ttu-id="49ba7-114">Állítsa be *https://ExpenseApp-contoso.msappproxy.net/login/login.aspx* , hello kezdőlap URL-cím toogive a felhasználók zökkenőmentes élményt.</span><span class="sxs-lookup"><span data-stu-id="49ba7-114">Set *https://ExpenseApp-contoso.msappproxy.net/login/login.aspx* as hello home page URL toogive your users a seamless experience.</span></span> 

>[!NOTE]
><span data-ttu-id="49ba7-115">Amikor a felhasználók hozzáférést toopublished alkalmazásokat adni, hello jelennek meg a hello alkalmazások [Azure AD hozzáférési Panel](active-directory-saas-access-panel-introduction.md) és hello [Office 365 alkalmazás indító](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).</span><span class="sxs-lookup"><span data-stu-id="49ba7-115">When you give users access toopublished apps, hello apps are displayed in hello [Azure AD Access Panel](active-directory-saas-access-panel-introduction.md) and hello [Office 365 app launcher](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="49ba7-116">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="49ba7-116">Before you start</span></span>

<span data-ttu-id="49ba7-117">Hello kezdőlap URL-Címének beállítása előtt tartsa szem előtt tartva hello követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="49ba7-117">Before you set hello home page URL, keep in mind hello following requirements:</span></span>

* <span data-ttu-id="49ba7-118">Győződjön meg arról, hogy hello megadott elérési út egy altartomány elérési utat az hello legfelső szintű tartomány URL-cím.</span><span class="sxs-lookup"><span data-stu-id="49ba7-118">Ensure that hello path you specify is a subdomain path of hello root domain URL.</span></span>

  <span data-ttu-id="49ba7-119">Ha hello gyökértartomány URL-CÍMÉT, például https://apps.contoso.com/app1/, hello kezdőlap URL-címet meg kell kezdődnie https://apps.contoso.com/app1/.</span><span class="sxs-lookup"><span data-stu-id="49ba7-119">If hello root-domain URL is, for example, https://apps.contoso.com/app1/, hello home page URL that you configure must start with https://apps.contoso.com/app1/.</span></span>

* <span data-ttu-id="49ba7-120">Ha olyan módosítást toohello közzétett alkalmazás, hello módosítása előfordulhat, hogy alaphelyzetbe hello értékének hello kezdőlap URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="49ba7-120">If you make a change toohello published app, hello change might reset hello value of hello home page URL.</span></span> <span data-ttu-id="49ba7-121">A jövőbeli hello hello alkalmazás frissítésekor kell újbóli ellenőrzéséhez és, ha szükséges, frissítse a hello kezdőlap URL-címe.</span><span class="sxs-lookup"><span data-stu-id="49ba7-121">When you update hello app in hello future, you should recheck and, if necessary, update hello home page URL.</span></span>

## <a name="change-hello-home-page-in-hello-azure-portal"></a><span data-ttu-id="49ba7-122">Hello kezdőlap módosítása a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="49ba7-122">Change hello home page in hello Azure portal</span></span>

1. <span data-ttu-id="49ba7-123">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="49ba7-123">Sign in toohello [Azure portal](https://portal.azure.com) as an administrator.</span></span>
2. <span data-ttu-id="49ba7-124">Keresse meg a túl**Azure Active Directory** > **App regisztrációk** , és válassza ki az alkalmazás hello listáról.</span><span class="sxs-lookup"><span data-stu-id="49ba7-124">Navigate too**Azure Active Directory** > **App registrations** and choose your application from hello list.</span></span> 
3. <span data-ttu-id="49ba7-125">Válassza ki **tulajdonságok** hello beállításai közül.</span><span class="sxs-lookup"><span data-stu-id="49ba7-125">Select **Properties** from hello settings.</span></span>
4. <span data-ttu-id="49ba7-126">Frissítés hello **Kezdőlap** mező az új elérési úttal.</span><span class="sxs-lookup"><span data-stu-id="49ba7-126">Update hello **Home page URL** field with your new path.</span></span> 

   ![Új kezdőlap URL-Címének megadása](./media/application-proxy-office365-app-launcher/homepage.png)

5. <span data-ttu-id="49ba7-128">Válassza ki **mentése**</span><span class="sxs-lookup"><span data-stu-id="49ba7-128">Select **Save**</span></span>

## <a name="change-hello-home-page-with-powershell"></a><span data-ttu-id="49ba7-129">Hello kezdőlap módosítása a Powershellel</span><span class="sxs-lookup"><span data-stu-id="49ba7-129">Change hello home page with PowerShell</span></span>

### <a name="install-hello-azure-ad-powershell-module"></a><span data-ttu-id="49ba7-130">Hello Azure AD PowerShell modul telepítése</span><span class="sxs-lookup"><span data-stu-id="49ba7-130">Install hello Azure AD PowerShell module</span></span>

<span data-ttu-id="49ba7-131">Egy egyéni kezdőlap URL-Címének megadása PowerShell segítségével, előtt telepítse az hello Azure AD PowerShell modult.</span><span class="sxs-lookup"><span data-stu-id="49ba7-131">Before you define a custom home page URL by using PowerShell, install hello Azure AD PowerShell module.</span></span> <span data-ttu-id="49ba7-132">Hello csomag letölthető hello [PowerShell-galériában](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131), mely által használt hello Graph API-végpont.</span><span class="sxs-lookup"><span data-stu-id="49ba7-132">You can download hello package from hello [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131), which uses hello Graph API endpoint.</span></span> 

<span data-ttu-id="49ba7-133">tooinstall hello csomag, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="49ba7-133">tooinstall hello package, follow these steps:</span></span>

1. <span data-ttu-id="49ba7-134">Nyissa meg a standard PowerShell-ablakot, és futtassa a parancsot a következő hello:</span><span class="sxs-lookup"><span data-stu-id="49ba7-134">Open a standard PowerShell window, and then run hello following command:</span></span>

    ```
     Install-Module -Name AzureAD
    ```
    <span data-ttu-id="49ba7-135">Ha hello parancsot futtatja, a nem rendszergazda, akkor hello `-scope currentuser` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="49ba7-135">If you're running hello command as a non-admin, use hello `-scope currentuser` option.</span></span>
2. <span data-ttu-id="49ba7-136">Hello a telepítés során válassza **Y** tooinstall két Nuget.org a csomagokat. Mindkét csomagot szükség.</span><span class="sxs-lookup"><span data-stu-id="49ba7-136">During hello installation, select **Y** tooinstall two packages from Nuget.org. Both packages are required.</span></span> 

### <a name="find-hello-objectid-of-hello-app"></a><span data-ttu-id="49ba7-137">Hello ObjectID hello alkalmazás keresése</span><span class="sxs-lookup"><span data-stu-id="49ba7-137">Find hello ObjectID of hello app</span></span>

<span data-ttu-id="49ba7-138">Hello ObjectID hello alkalmazás beszerzése, és keressen a hello alkalmazás által a kezdőlap.</span><span class="sxs-lookup"><span data-stu-id="49ba7-138">Obtain hello ObjectID of hello app, and then search for hello app by its home page.</span></span>

1. <span data-ttu-id="49ba7-139">Nyissa meg a Powershellt, és az Azure AD hello modul importálásához.</span><span class="sxs-lookup"><span data-stu-id="49ba7-139">Open PowerShell and import hello Azure AD module.</span></span>

    ```
    Import-Module AzureAD
    ```

2. <span data-ttu-id="49ba7-140">Jelentkezzen be toohello az Azure AD-modullal hello bérlői rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="49ba7-140">Sign in toohello Azure AD module as hello tenant administrator.</span></span>

    ```
    Connect-AzureAD
    ```
3. <span data-ttu-id="49ba7-141">A kezdőlap URL-címe alapján hello alkalmazás megkereséséhez.</span><span class="sxs-lookup"><span data-stu-id="49ba7-141">Find hello app based on its home page URL.</span></span> <span data-ttu-id="49ba7-142">Hello URL-címet az található hello portal túl címen**Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="49ba7-142">You can find hello URL in hello portal by going too**Azure Active Directory** > **Enterprise applications** > **All applications**.</span></span> <span data-ttu-id="49ba7-143">Ez a példa *sharepoint-iddemo*.</span><span class="sxs-lookup"><span data-stu-id="49ba7-143">This example uses *sharepoint-iddemo*.</span></span>

    ```
    Get-AzureADApplication | where { $_.Homepage -like “sharepoint-iddemo” } | fl DisplayName, Homepage, ObjectID
    ```
4. <span data-ttu-id="49ba7-144">Amely hasonló toohello itt látható egy eredményt kapja meg.</span><span class="sxs-lookup"><span data-stu-id="49ba7-144">You should get a result that's similar toohello one shown here.</span></span> <span data-ttu-id="49ba7-145">Másolja a hello ObjectID GUID toouse hello a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="49ba7-145">Copy hello ObjectID GUID toouse in hello next section.</span></span>

    ```
    DisplayName : SharePoint
    Homepage    : https://sharepoint-iddemo.msappproxy.net/
    ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

### <a name="update-hello-home-page-url"></a><span data-ttu-id="49ba7-146">Hello kezdőlap URL-Címének frissítése</span><span class="sxs-lookup"><span data-stu-id="49ba7-146">Update hello home page URL</span></span>

<span data-ttu-id="49ba7-147">1. lépésben használt ugyanazon PowerShell-modul hello elvégezni hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="49ba7-147">In hello same PowerShell module that you used for step 1, perform hello following steps:</span></span>

1. <span data-ttu-id="49ba7-148">Győződjön meg arról, hogy rendelkezik-e hello javítsa ki az alkalmazást, és cserélje le *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* a hello ObjectID hello az előző lépésben másolt.</span><span class="sxs-lookup"><span data-stu-id="49ba7-148">Confirm that you have hello correct app, and replace *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* with hello ObjectID that you copied in hello preceding step.</span></span>

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4.
    ```

 <span data-ttu-id="49ba7-149">Most, hogy hello app megerősítését most készen áll a tooupdate hello kezdőlap, az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="49ba7-149">Now that you've confirmed hello app, you're ready tooupdate hello home page, as follows.</span></span>

2. <span data-ttu-id="49ba7-150">Hozzon létre egy üres alkalmazás objektum toohold, amelyet az toomake hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="49ba7-150">Create a blank application object toohold hello changes that you want toomake.</span></span> <span data-ttu-id="49ba7-151">Ezt a változót, amelyet az tooupdate hello értékeket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="49ba7-151">This variable holds hello values that you want tooupdate.</span></span> <span data-ttu-id="49ba7-152">Ezt a lépést nem jön létre.</span><span class="sxs-lookup"><span data-stu-id="49ba7-152">Nothing is created in this step.</span></span>

    ```
    $appnew = New-Object “Microsoft.Open.AzureAD.Model.Application”
    ```

3. <span data-ttu-id="49ba7-153">Hello kezdőlap URL-cím toohello érték, amelyet állítható be.</span><span class="sxs-lookup"><span data-stu-id="49ba7-153">Set hello home page URL toohello value that you want.</span></span> <span data-ttu-id="49ba7-154">hello érték lehet egy altartomány hello közzétett alkalmazás elérési útját.</span><span class="sxs-lookup"><span data-stu-id="49ba7-154">hello value must be a subdomain path of hello published app.</span></span> <span data-ttu-id="49ba7-155">Például, ha módosítja hello kezdőlap URL-CÍMÉT *https://sharepoint-iddemo.msappproxy.net/* túl*https://sharepoint-iddemo.msappproxy.net/hybrid/*, az alkalmazásaik felhasználóit lépjen közvetlenül toohello egyéni Kezdőlap.</span><span class="sxs-lookup"><span data-stu-id="49ba7-155">For example, if you change hello home page URL from *https://sharepoint-iddemo.msappproxy.net/* too*https://sharepoint-iddemo.msappproxy.net/hybrid/*, app users go directly toohello custom home page.</span></span>

    ```
    $homepage = “https://sharepoint-iddemo.msappproxy.net/hybrid/”
    ```
4. <span data-ttu-id="49ba7-156">Ellenőrizze a frissítés hello hello GUID azonosítója (ObjectID) a használatával "1. lépés: keresés hello hello alkalmazás ObjectID."</span><span class="sxs-lookup"><span data-stu-id="49ba7-156">Make hello update by using hello GUID (ObjectID) that you copied in "Step 1: Find hello ObjectID of hello app."</span></span>

    ```
    Set-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4 -Homepage $homepage
    ```
5. <span data-ttu-id="49ba7-157">tooconfirm, hogy hello módosítása sikeres volt, indítsa újra a hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="49ba7-157">tooconfirm that hello change was successful, restart hello app.</span></span>

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

>[!NOTE]
><span data-ttu-id="49ba7-158">Bármely végrehajtott változtatásokat toohello app előfordulhat, hogy alaphelyzetbe hello kezdőlap URL-címe.</span><span class="sxs-lookup"><span data-stu-id="49ba7-158">Any changes that you make toohello app might reset hello home page URL.</span></span> <span data-ttu-id="49ba7-159">Ha alaphelyzetbe állítja a kezdőlap URL-CÍMÉT, ismételje meg a 2.</span><span class="sxs-lookup"><span data-stu-id="49ba7-159">If your home page URL resets, repeat step 2.</span></span>

## <a name="next-steps"></a><span data-ttu-id="49ba7-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="49ba7-160">Next steps</span></span>

- [<span data-ttu-id="49ba7-161">Távelérés tooSharePoint az Azure AD alkalmazásproxy engedélyezése</span><span class="sxs-lookup"><span data-stu-id="49ba7-161">Enable remote access tooSharePoint with Azure AD Application Proxy</span></span>](application-proxy-enable-remote-access-sharepoint.md)
- [<span data-ttu-id="49ba7-162">Alkalmazásproxy engedélyezése az Azure-portálon hello</span><span class="sxs-lookup"><span data-stu-id="49ba7-162">Enable Application Proxy in hello Azure portal</span></span>](active-directory-application-proxy-enable.md)
