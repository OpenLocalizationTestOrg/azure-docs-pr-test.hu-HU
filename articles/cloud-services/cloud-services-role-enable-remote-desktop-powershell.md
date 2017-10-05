---
title: "Azure Cloud Services PowerShell használatával a szerepkör távoli asztali kapcsolat engedélyezése"
description: "Konfigurálása az azure cloud service alkalmazás távoli asztali kapcsolatok engedélyezése a PowerShell használatával"
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: bf2f70a4-20dc-4302-a91a-38cd7a2baa62
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 171f27c92ee9de14301ebb664e9ba3bcd98c394d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a><span data-ttu-id="98e84-103">Azure Cloud Services PowerShell használatával a szerepkör távoli asztali kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="98e84-103">Enable Remote Desktop Connection for a Role in Azure Cloud Services using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="98e84-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="98e84-104">Azure portal</span></span>](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [<span data-ttu-id="98e84-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="98e84-105">Azure classic portal</span></span>](cloud-services-role-enable-remote-desktop.md)
> * [<span data-ttu-id="98e84-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="98e84-106">PowerShell</span></span>](cloud-services-role-enable-remote-desktop-powershell.md)
> * [<span data-ttu-id="98e84-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98e84-107">Visual Studio</span></span>](../vs-azure-tools-remote-desktop-roles.md)
>
>

<span data-ttu-id="98e84-108">A távoli asztal lehetővé teszi az asztalon, egy Azure-beli szerepkör eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="98e84-108">Remote Desktop enables you to access the desktop of a role running in Azure.</span></span> <span data-ttu-id="98e84-109">A távoli asztali kapcsolat segítségével hibaelhárítását és diagnosztizálását az alkalmazás futtatása során.</span><span class="sxs-lookup"><span data-stu-id="98e84-109">You can use a Remote Desktop connection to troubleshoot and diagnose problems with your application while it is running.</span></span>

<span data-ttu-id="98e84-110">Ez a cikk ismerteti a távoli asztal engedélyezése a PowerShell használatával a Felhőszolgáltatás szerepköreit.</span><span class="sxs-lookup"><span data-stu-id="98e84-110">This article describes how to enable remote desktop on your Cloud Service Roles using PowerShell.</span></span> <span data-ttu-id="98e84-111">Lásd: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview) esetében ez a cikk szükséges előfeltételeket.</span><span class="sxs-lookup"><span data-stu-id="98e84-111">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for the prerequisites needed for this article.</span></span> <span data-ttu-id="98e84-112">PowerShell használja a távoli asztali bővítmény, ahol engedélyezheti a távoli asztal, az alkalmazás telepítése után.</span><span class="sxs-lookup"><span data-stu-id="98e84-112">PowerShell utilizes the Remote Desktop Extension so you can enable Remote Desktop after the application is deployed.</span></span>

## <a name="configure-remote-desktop-from-powershell"></a><span data-ttu-id="98e84-113">A PowerShell távoli asztal konfigurálása</span><span class="sxs-lookup"><span data-stu-id="98e84-113">Configure Remote Desktop from PowerShell</span></span>
<span data-ttu-id="98e84-114">A [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) parancsmag lehetővé teszi a távoli asztal engedélyezése a megadott szerepkörök vagy a cloud service-környezet minden szerepkört.</span><span class="sxs-lookup"><span data-stu-id="98e84-114">The [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet allows you to enable Remote Desktop on specified roles or all roles of your cloud service deployment.</span></span> <span data-ttu-id="98e84-115">A parancsmag lehetővé teszi, hogy a felhasználónév és jelszó megadása a távoli asztali felhasználó leállította a *Credential* paraméter, amely fogad egy PSCredential objektumot.</span><span class="sxs-lookup"><span data-stu-id="98e84-115">The cmdlet lets you specify the Username and Password for the remote desktop user through the *Credential* parameter that accepts a PSCredential object.</span></span>

<span data-ttu-id="98e84-116">Ha PowerShell interaktív módon használja, egyszerűen beállíthatja a PSCredential objektum meghívásával a [Get-hitelesítő adatok](https://technet.microsoft.com/library/hh849815.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="98e84-116">If you are using PowerShell interactively, you can easily set the PSCredential object by calling the [Get-Credentials](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span></span>

```
$remoteusercredentials = Get-Credential
```

<span data-ttu-id="98e84-117">Ez a parancs megjelenít egy párbeszédpanelt, hogy lehetővé teszi a biztonságos módon a távoli felhasználót a felhasználónév és jelszó megadása.</span><span class="sxs-lookup"><span data-stu-id="98e84-117">This command displays a dialog box allowing you to enter the username and password for the remote user in a secure manner.</span></span>

<span data-ttu-id="98e84-118">Automation-forgatókönyvek PowerShell hozzájárul, mivel is beállíthat a **PSCredential** objektum oly módon, hogy nincs szükség felhasználói beavatkozásra.</span><span class="sxs-lookup"><span data-stu-id="98e84-118">Since PowerShell helps in automation scenarios, you can also set up the **PSCredential** object in a way that doesn't require user interaction.</span></span> <span data-ttu-id="98e84-119">Először akkor be kell állítania egy biztonságos jelszó.</span><span class="sxs-lookup"><span data-stu-id="98e84-119">First, you need to set up a secure password.</span></span> <span data-ttu-id="98e84-120">Egy egyszerű szöveges jelszó átalakítása a egy biztonságos karakterláncot használó megadásával megkezdése [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span><span class="sxs-lookup"><span data-stu-id="98e84-120">You begin with specifying a plain text password convert it to a secure string using [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span></span> <span data-ttu-id="98e84-121">Ezután kell konvertálni a biztonságos karakterláncot kell megadnia egy titkosított szabványos karakterláncok használatával [ConvertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx).</span><span class="sxs-lookup"><span data-stu-id="98e84-121">Next you need to convert this secure string into an encrypted standard string using [ConvertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx).</span></span> <span data-ttu-id="98e84-122">Most egy fájl használatával mentheti a titkosított szabványos karakterlánc [Set-tartalom](https://technet.microsoft.com/library/ee176959.aspx).</span><span class="sxs-lookup"><span data-stu-id="98e84-122">Now you can save this encrypted standard string to a file using [Set-Content](https://technet.microsoft.com/library/ee176959.aspx).</span></span>

<span data-ttu-id="98e84-123">Biztonságos jelszó fájl is létrehozhat, így nem kell minden alkalommal meg a jelszót.</span><span class="sxs-lookup"><span data-stu-id="98e84-123">You can also create a secure password file so that you don't have to type in the password every time.</span></span> <span data-ttu-id="98e84-124">Biztonságos jelszó fájl is nagyobb, mint egy egyszerű szöveges fájl.</span><span class="sxs-lookup"><span data-stu-id="98e84-124">Also, a secure password file is better than a plain text file.</span></span> <span data-ttu-id="98e84-125">A következő PowerShell segítségével hozzon létre egy biztonságos jelszó fájlt:</span><span class="sxs-lookup"><span data-stu-id="98e84-125">Use the following PowerShell to create a secure password file:</span></span>

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

> [!IMPORTANT]
> <span data-ttu-id="98e84-126">Amikor a jelszó beállítását, győződjön meg arról, hogy teljesülnek a [bonyolult](https://technet.microsoft.com/library/cc786468.aspx).</span><span class="sxs-lookup"><span data-stu-id="98e84-126">When setting the password, make sure that you meet the [complexity requirements](https://technet.microsoft.com/library/cc786468.aspx).</span></span>
>
>

<span data-ttu-id="98e84-127">A hitelesítő objektum létrehozása a biztonságos jelszó fájlból, kell a fájl tartalmának olvasása és alakíthatja át őket egy biztonságos karakterláncot használni [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span><span class="sxs-lookup"><span data-stu-id="98e84-127">To create the credential object from the secure password file, you must read the file contents and convert them back to a secure string using [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span></span>

<span data-ttu-id="98e84-128">A [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) parancsmag is fogad el egy *lejárati* paraméter, amely megadja egy **DateTime** , amely a felhasználói fiók lejár.</span><span class="sxs-lookup"><span data-stu-id="98e84-128">The [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet also accepts an *Expiration* parameter, which specifies a **DateTime** at which the user account expires.</span></span> <span data-ttu-id="98e84-129">Például beállíthat a fiók az aktuális dátum és idő néhány nap múlva lejár.</span><span class="sxs-lookup"><span data-stu-id="98e84-129">For example, you could set the account to expire a few days from the current date and time.</span></span>

<span data-ttu-id="98e84-130">A PowerShell-példa bemutatja, hogyan állítsa be a távoli asztali bővítmény egy felhőalapú szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="98e84-130">This PowerShell example shows you how to set the Remote Desktop Extension on a cloud service:</span></span>

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
<span data-ttu-id="98e84-131">Opcionálisan kiegészítheti az üzembe helyezési pont és a távoli asztal engedélyezése a kívánt szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="98e84-131">You can also optionally specify the deployment slot and roles that you want to enable remote desktop on.</span></span> <span data-ttu-id="98e84-132">Ha ezek a paraméterek nincsenek megadva, a parancsmag lehetővé teszi, hogy az összes szerepkör a távoli asztal a **éles** üzembe helyezési pont.</span><span class="sxs-lookup"><span data-stu-id="98e84-132">If these parameters are not specified, the cmdlet enables remote desktop on all roles in the **Production** deployment slot.</span></span>

<span data-ttu-id="98e84-133">A távoli asztali bővítmény társítva a központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="98e84-133">The Remote Desktop extension is associated with a deployment.</span></span> <span data-ttu-id="98e84-134">Ha létrehoz egy új központi telepítést, a szolgáltatás számára, hogy távoli asztal engedélyezése a központi telepítést.</span><span class="sxs-lookup"><span data-stu-id="98e84-134">If you create a new deployment for the service, you have to enable remote desktop on that deployment.</span></span> <span data-ttu-id="98e84-135">Ha azt szeretné, hogy az engedélyezve van a távoli asztal, majd vegye figyelembe a PowerShell-parancsfájlok integrálása a telepítési munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="98e84-135">If you always want to have remote desktop enabled, then you should consider integrating the PowerShell scripts into your deployment workflow.</span></span>

## <a name="remote-desktop-into-a-role-instance"></a><span data-ttu-id="98e84-136">Távoli asztali kapcsolatot egy szerepkörpéldányt</span><span class="sxs-lookup"><span data-stu-id="98e84-136">Remote Desktop into a role instance</span></span>
<span data-ttu-id="98e84-137">A [Get-AzureRemoteDesktopFile](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) parancsmag használatban a távoli asztali kapcsolatot egy adott szerepkör példánya a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="98e84-137">The [Get-AzureRemoteDesktopFile](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet is used to remote desktop into a specific role instance of your cloud service.</span></span> <span data-ttu-id="98e84-138">Használhatja a *LocalPath* paraméter töltse le az RDP a fájlt helyileg.</span><span class="sxs-lookup"><span data-stu-id="98e84-138">You can use the *LocalPath* parameter to download the RDP file locally.</span></span> <span data-ttu-id="98e84-139">Vagy használhatja a *indítása* paraméter segítségével közvetlenül indítsa el a távoli asztali kapcsolat párbeszédpanel a felhőalapú szolgáltatás szerepkör példánya eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="98e84-139">Or you can use the *Launch* parameter to directly launch the Remote Desktop Connection dialog to access the cloud service role instance.</span></span>

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a><span data-ttu-id="98e84-140">Ellenőrizze, hogy engedélyezve van-e a távoli asztal bővítmény egy szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="98e84-140">Check if Remote Desktop extension is enabled on a service</span></span>
<span data-ttu-id="98e84-141">A [Get-AzureServiceRemoteDesktopExtension](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) parancsmag jeleníti meg, hogy a távoli asztal engedélyezve van-e a szolgáltatás üzemelő példányon.</span><span class="sxs-lookup"><span data-stu-id="98e84-141">The [Get-AzureServiceRemoteDesktopExtension](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet displays that remote desktop is enabled or disabled on a service deployment.</span></span> <span data-ttu-id="98e84-142">A parancsmag visszaadja a felhasználónév, a távoli asztal felhasználóját, és a szerepköröket, amelyek a távoli asztali bővítmény engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="98e84-142">The cmdlet returns the username for the remote desktop user and the roles that the remote desktop extension is enabled for.</span></span> <span data-ttu-id="98e84-143">Alapértelmezés szerint ez az üzembe helyezési pont történik, és esetén dönthet úgy, hogy az átmeneti helyet használja helyette.</span><span class="sxs-lookup"><span data-stu-id="98e84-143">By default, this happens on the deployment slot and you can choose to use the staging slot instead.</span></span>

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a><span data-ttu-id="98e84-144">A szolgáltatás a távoli asztal-kiterjesztés eltávolítása</span><span class="sxs-lookup"><span data-stu-id="98e84-144">Remove Remote Desktop extension from a service</span></span>
<span data-ttu-id="98e84-145">Ha már engedélyezve van a távoli asztali bővítmény üzemelő példányon, és frissítenie kell a távoli asztal beállításait, először távolítsa el a bővítményt.</span><span class="sxs-lookup"><span data-stu-id="98e84-145">If you have already enabled the remote desktop extension on a deployment, and need to update the remote desktop settings, first remove the extension.</span></span> <span data-ttu-id="98e84-146">Majd engedélyezze újra az új beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="98e84-146">And enable it again with the new settings.</span></span> <span data-ttu-id="98e84-147">Ha például azt szeretné, hogy a távoli felhasználói fiókhoz tartozó új jelszót állíthat be, vagy a fiók lejárt.</span><span class="sxs-lookup"><span data-stu-id="98e84-147">For example, if you want to set a new password for the remote user account, or the account expired.</span></span> <span data-ttu-id="98e84-148">Ezzel a meglévő telepítések engedélyezve van a távoli asztali kiterjesztésű szükséges.</span><span class="sxs-lookup"><span data-stu-id="98e84-148">Doing this is required on existing deployments that have the remote desktop extension enabled.</span></span> <span data-ttu-id="98e84-149">Az új központi telepítéseknél egyszerűen alkalmazhatja a bővítmény közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="98e84-149">For new deployments, you can simply apply the extension directly.</span></span>

<span data-ttu-id="98e84-150">A központi telepítést a távoli asztali bővítmény eltávolításához használja a [Remove-AzureServiceRemoteDesktopExtension](/powershell/module/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="98e84-150">To remove the remote desktop extension from the deployment, you can use the [Remove-AzureServiceRemoteDesktopExtension](/powershell/module/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet.</span></span> <span data-ttu-id="98e84-151">Opcionálisan kiegészítheti az üzembe helyezési pont és a szerepkör, amelyről el szeretné távolítani a távoli asztali bővítmény.</span><span class="sxs-lookup"><span data-stu-id="98e84-151">You can also optionally specify the deployment slot and role from which you want to remove the remote desktop extension.</span></span>

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

> [!NOTE]
> <span data-ttu-id="98e84-152">A bővítmény konfigurációja teljes eltávolításához meg kell hívnia a *eltávolítása* parancsmagot a **UninstallConfiguration** paraméter.</span><span class="sxs-lookup"><span data-stu-id="98e84-152">To completely remove the extension configuration, you should call the *remove* cmdlet with the **UninstallConfiguration** parameter.</span></span>
>
> <span data-ttu-id="98e84-153">A **UninstallConfiguration** paraméter eltávolítja a szolgáltatás alkalmazott bővítmény beállításra.</span><span class="sxs-lookup"><span data-stu-id="98e84-153">The **UninstallConfiguration** parameter uninstalls any extension configuration that is applied to the service.</span></span> <span data-ttu-id="98e84-154">Minden bővítménykonfiguráció nem tartozik a szolgáltatás konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="98e84-154">Every extension configuration is associated with the service configuration.</span></span> <span data-ttu-id="98e84-155">Hívja a *eltávolítása* nélkül parancsmag **UninstallConfiguration** megszünteti a <mark>telepítési</mark> a bővítménykonfiguráció, így gyakorlatilag kivonja a a bővítmény.</span><span class="sxs-lookup"><span data-stu-id="98e84-155">Calling the *remove* cmdlet without **UninstallConfiguration** disassociates the <mark>deployment</mark> from the extension configuration, thus effectively removing the extension.</span></span> <span data-ttu-id="98e84-156">Azonban a bővítmény konfigurációja továbbra is társítva van a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="98e84-156">However, the extension configuration remains associated with the service.</span></span>
>
>

## <a name="additional-resources"></a><span data-ttu-id="98e84-157">További források</span><span class="sxs-lookup"><span data-stu-id="98e84-157">Additional resources</span></span>

<span data-ttu-id="98e84-158">[Felhőalapú szolgáltatások konfigurálása](cloud-services-how-to-configure.md)
[Cloud services – gyakori kérdések – a távoli asztal](cloud-services-faq.md)</span><span class="sxs-lookup"><span data-stu-id="98e84-158">[How to Configure Cloud Services](cloud-services-how-to-configure.md)
[Cloud services FAQ - Remote Desktop](cloud-services-faq.md)</span></span>
