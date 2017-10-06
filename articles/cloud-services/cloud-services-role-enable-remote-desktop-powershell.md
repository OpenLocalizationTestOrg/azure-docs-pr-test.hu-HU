---
title: "Azure Cloud Services PowerShell használatával a szerepkör távoli asztali kapcsolat aaaEnable"
description: "Hogyan tooconfigure az azure cloud service alkalmazás PowerShell tooallow távoli asztali kapcsolaton"
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
ms.openlocfilehash: 3f46b014f29f1c0be0e1b485d2f0152424162bb2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a><span data-ttu-id="f7736-103">Azure Cloud Services PowerShell használatával a szerepkör távoli asztali kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f7736-103">Enable Remote Desktop Connection for a Role in Azure Cloud Services using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f7736-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f7736-104">Azure portal</span></span>](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [<span data-ttu-id="f7736-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="f7736-105">Azure classic portal</span></span>](cloud-services-role-enable-remote-desktop.md)
> * [<span data-ttu-id="f7736-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f7736-106">PowerShell</span></span>](cloud-services-role-enable-remote-desktop-powershell.md)
> * [<span data-ttu-id="f7736-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f7736-107">Visual Studio</span></span>](../vs-azure-tools-remote-desktop-roles.md)
>
>

<span data-ttu-id="f7736-108">A távoli asztal tooaccess hello asztali egy Azure-beli szerepkör lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="f7736-108">Remote Desktop enables you tooaccess hello desktop of a role running in Azure.</span></span> <span data-ttu-id="f7736-109">A távoli asztali kapcsolat tootroubleshoot használja, és hibáinak az alkalmazás futtatása során.</span><span class="sxs-lookup"><span data-stu-id="f7736-109">You can use a Remote Desktop connection tootroubleshoot and diagnose problems with your application while it is running.</span></span>

<span data-ttu-id="f7736-110">Ez a cikk ismerteti, hogyan tooenable távoli asztal a PowerShell használatával a Felhőszolgáltatás szerepköreit.</span><span class="sxs-lookup"><span data-stu-id="f7736-110">This article describes how tooenable remote desktop on your Cloud Service Roles using PowerShell.</span></span> <span data-ttu-id="f7736-111">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) Ez a cikk szükséges hello előfeltételeinek.</span><span class="sxs-lookup"><span data-stu-id="f7736-111">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for hello prerequisites needed for this article.</span></span> <span data-ttu-id="f7736-112">PowerShell használja a távoli asztali bővítmény hello, ahol engedélyezheti a távoli asztal, hello alkalmazás telepítése után.</span><span class="sxs-lookup"><span data-stu-id="f7736-112">PowerShell utilizes hello Remote Desktop Extension so you can enable Remote Desktop after hello application is deployed.</span></span>

## <a name="configure-remote-desktop-from-powershell"></a><span data-ttu-id="f7736-113">A PowerShell távoli asztal konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f7736-113">Configure Remote Desktop from PowerShell</span></span>
<span data-ttu-id="f7736-114">Hello [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) parancsmag lehetővé teszi a távoli asztal tooenable megadott szerepkörök vagy a cloud service-környezet minden szerepkört.</span><span class="sxs-lookup"><span data-stu-id="f7736-114">hello [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet allows you tooenable Remote Desktop on specified roles or all roles of your cloud service deployment.</span></span> <span data-ttu-id="f7736-115">hello parancsmag lehetővé teszi, hogy adja meg az hello távoli asztal felhasználójának keresztül hello hello felhasználónév és jelszó *Credential* paraméter, amely fogad egy PSCredential objektumot.</span><span class="sxs-lookup"><span data-stu-id="f7736-115">hello cmdlet lets you specify hello Username and Password for hello remote desktop user through hello *Credential* parameter that accepts a PSCredential object.</span></span>

<span data-ttu-id="f7736-116">Ha PowerShell interaktív módon használja, egyszerűen beállíthatja hello PSCredential objektum által hívó hello [Get-hitelesítő adatok](https://technet.microsoft.com/library/hh849815.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="f7736-116">If you are using PowerShell interactively, you can easily set hello PSCredential object by calling hello [Get-Credentials](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span></span>

```
$remoteusercredentials = Get-Credential
```

<span data-ttu-id="f7736-117">Ez a parancs megjelenít egy párbeszédpanelt, így Ön tooenter hello felhasználónév és a jelszó hello távoli biztonságos módon.</span><span class="sxs-lookup"><span data-stu-id="f7736-117">This command displays a dialog box allowing you tooenter hello username and password for hello remote user in a secure manner.</span></span>

<span data-ttu-id="f7736-118">Mivel a PowerShell segítségével az automation-forgatókönyvek, is beállíthat hello **PSCredential** objektum oly módon, hogy nincs szükség felhasználói beavatkozásra.</span><span class="sxs-lookup"><span data-stu-id="f7736-118">Since PowerShell helps in automation scenarios, you can also set up hello **PSCredential** object in a way that doesn't require user interaction.</span></span> <span data-ttu-id="f7736-119">Először tooset biztonságos jelszó.</span><span class="sxs-lookup"><span data-stu-id="f7736-119">First, you need tooset up a secure password.</span></span> <span data-ttu-id="f7736-120">Egy egyszerű szöveges jelszó átalakíthatja a tooa biztonságos karakterlánc megadását előtaggal használatával [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7736-120">You begin with specifying a plain text password convert it tooa secure string using [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span></span> <span data-ttu-id="f7736-121">Ezután kell tooconvert a biztonságos karakterláncot kell megadnia egy titkosított szabványos karakterláncok használatával történő [ConvertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7736-121">Next you need tooconvert this secure string into an encrypted standard string using [ConvertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx).</span></span> <span data-ttu-id="f7736-122">Most a titkosított szabványos karakterlánc tooa fájl használatával mentheti [Set-tartalom](https://technet.microsoft.com/library/ee176959.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7736-122">Now you can save this encrypted standard string tooa file using [Set-Content](https://technet.microsoft.com/library/ee176959.aspx).</span></span>

<span data-ttu-id="f7736-123">A biztonságos jelszó fájlt, hogy nincs tootype hello jelszó minden alkalommal is létrehozhatja.</span><span class="sxs-lookup"><span data-stu-id="f7736-123">You can also create a secure password file so that you don't have tootype in hello password every time.</span></span> <span data-ttu-id="f7736-124">Biztonságos jelszó fájl is nagyobb, mint egy egyszerű szöveges fájl.</span><span class="sxs-lookup"><span data-stu-id="f7736-124">Also, a secure password file is better than a plain text file.</span></span> <span data-ttu-id="f7736-125">A következő PowerShell toocreate biztonságos jelszó fájl hello használata:</span><span class="sxs-lookup"><span data-stu-id="f7736-125">Use hello following PowerShell toocreate a secure password file:</span></span>

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

> [!IMPORTANT]
> <span data-ttu-id="f7736-126">Hello jelszó beállításakor győződjön meg arról, hogy teljesülnek-e hello [bonyolult](https://technet.microsoft.com/library/cc786468.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7736-126">When setting hello password, make sure that you meet hello [complexity requirements](https://technet.microsoft.com/library/cc786468.aspx).</span></span>
>
>

<span data-ttu-id="f7736-127">toocreate hello hitelesítő objektum hello biztonságos jelszó fájlból kell hello fájl tartalmának olvasása és alakíthatja át őket hátsó tooa biztonságos karakterláncot [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7736-127">toocreate hello credential object from hello secure password file, you must read hello file contents and convert them back tooa secure string using [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span></span>

<span data-ttu-id="f7736-128">Hello [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) parancsmag is fogad el egy *lejárati* paraméter, amely megadja egy **DateTime** hello felhasználói fiók lejár.</span><span class="sxs-lookup"><span data-stu-id="f7736-128">hello [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet also accepts an *Expiration* parameter, which specifies a **DateTime** at which hello user account expires.</span></span> <span data-ttu-id="f7736-129">Például beállíthat hello fiók tooexpire néhány nap múlva az aktuális dátum és idő hello.</span><span class="sxs-lookup"><span data-stu-id="f7736-129">For example, you could set hello account tooexpire a few days from hello current date and time.</span></span>

<span data-ttu-id="f7736-130">A PowerShell-példa bemutatja, hogyan tooset hello egy felhőalapú szolgáltatás a távoli asztali bővítmény:</span><span class="sxs-lookup"><span data-stu-id="f7736-130">This PowerShell example shows you how tooset hello Remote Desktop Extension on a cloud service:</span></span>

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
<span data-ttu-id="f7736-131">Opcionálisan kiegészítheti hello üzembe helyezési pont és a szerepköröket, amelyek a távoli asztal tooenable szeretné.</span><span class="sxs-lookup"><span data-stu-id="f7736-131">You can also optionally specify hello deployment slot and roles that you want tooenable remote desktop on.</span></span> <span data-ttu-id="f7736-132">Ha ezek a paraméterek nincsenek megadva, hello parancsmag lehetővé teszi, hogy minden szerepkörök hello a távoli asztal **éles** üzembe helyezési pont.</span><span class="sxs-lookup"><span data-stu-id="f7736-132">If these parameters are not specified, hello cmdlet enables remote desktop on all roles in hello **Production** deployment slot.</span></span>

<span data-ttu-id="f7736-133">Távoli asztali bővítmény hello társítva a központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="f7736-133">hello Remote Desktop extension is associated with a deployment.</span></span> <span data-ttu-id="f7736-134">Ha létrehoz egy új hello szolgáltatás központi telepítését, tooenable távoli asztal van központi telepítést.</span><span class="sxs-lookup"><span data-stu-id="f7736-134">If you create a new deployment for hello service, you have tooenable remote desktop on that deployment.</span></span> <span data-ttu-id="f7736-135">Ha azt szeretné, hogy engedélyezve van a távoli asztal toohave, majd érdemes hello PowerShell-parancsfájlok integrálása a telepítési munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="f7736-135">If you always want toohave remote desktop enabled, then you should consider integrating hello PowerShell scripts into your deployment workflow.</span></span>

## <a name="remote-desktop-into-a-role-instance"></a><span data-ttu-id="f7736-136">Távoli asztali kapcsolatot egy szerepkörpéldányt</span><span class="sxs-lookup"><span data-stu-id="f7736-136">Remote Desktop into a role instance</span></span>
<span data-ttu-id="f7736-137">Hello [Get-AzureRemoteDesktopFile](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) parancsmag használt tooremote asztali kapcsolatot egy adott szerepkör példánya a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f7736-137">hello [Get-AzureRemoteDesktopFile](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet is used tooremote desktop into a specific role instance of your cloud service.</span></span> <span data-ttu-id="f7736-138">Használhatja a hello *LocalPath* paraméter toodownload hello RDP helyileg a fájlt.</span><span class="sxs-lookup"><span data-stu-id="f7736-138">You can use hello *LocalPath* parameter toodownload hello RDP file locally.</span></span> <span data-ttu-id="f7736-139">Vagy használhat hello *indítása* paraméter toodirectly indítási hello távoli asztali kapcsolat párbeszédpanel tooaccess hello felhőalapú szolgáltatás szerepkör példánya.</span><span class="sxs-lookup"><span data-stu-id="f7736-139">Or you can use hello *Launch* parameter toodirectly launch hello Remote Desktop Connection dialog tooaccess hello cloud service role instance.</span></span>

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a><span data-ttu-id="f7736-140">Ellenőrizze, hogy engedélyezve van-e a távoli asztal bővítmény egy szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f7736-140">Check if Remote Desktop extension is enabled on a service</span></span>
<span data-ttu-id="f7736-141">Hello [Get-AzureServiceRemoteDesktopExtension](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) parancsmag jeleníti meg, hogy a távoli asztal engedélyezve van-e a szolgáltatás üzemelő példányon.</span><span class="sxs-lookup"><span data-stu-id="f7736-141">hello [Get-AzureServiceRemoteDesktopExtension](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet displays that remote desktop is enabled or disabled on a service deployment.</span></span> <span data-ttu-id="f7736-142">hello parancsmag hello távoli asztal felhasználójának és a távoli asztali bővítmény hello engedélyezett hello szerepkörök hello felhasználónevét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="f7736-142">hello cmdlet returns hello username for hello remote desktop user and hello roles that hello remote desktop extension is enabled for.</span></span> <span data-ttu-id="f7736-143">Alapértelmezés szerint ez hello üzembe helyezési pont történik, és kiválaszthatja a tárolóhely ehelyett átmeneti toouse hello.</span><span class="sxs-lookup"><span data-stu-id="f7736-143">By default, this happens on hello deployment slot and you can choose toouse hello staging slot instead.</span></span>

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a><span data-ttu-id="f7736-144">A szolgáltatás a távoli asztal-kiterjesztés eltávolítása</span><span class="sxs-lookup"><span data-stu-id="f7736-144">Remove Remote Desktop extension from a service</span></span>
<span data-ttu-id="f7736-145">Ha hello távoli asztali bővítmény üzemelő példányon már engedélyezve van, és kell tooupdate hello távoli asztal beállításait, először távolítsa el a hello bővítmény.</span><span class="sxs-lookup"><span data-stu-id="f7736-145">If you have already enabled hello remote desktop extension on a deployment, and need tooupdate hello remote desktop settings, first remove hello extension.</span></span> <span data-ttu-id="f7736-146">Majd engedélyezze újra az új beállítások hello.</span><span class="sxs-lookup"><span data-stu-id="f7736-146">And enable it again with hello new settings.</span></span> <span data-ttu-id="f7736-147">Például ha azt szeretné, hogy tooset hello távoli felhasználói fiókot, vagy hello fiókot új jelszó lejárt.</span><span class="sxs-lookup"><span data-stu-id="f7736-147">For example, if you want tooset a new password for hello remote user account, or hello account expired.</span></span> <span data-ttu-id="f7736-148">Ezzel a meglévő központi telepítésben hello távoli asztali engedélyezett bővítményekhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="f7736-148">Doing this is required on existing deployments that have hello remote desktop extension enabled.</span></span> <span data-ttu-id="f7736-149">Az új központi telepítéseknél egyszerűen alkalmazhat hello bővítmény közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="f7736-149">For new deployments, you can simply apply hello extension directly.</span></span>

<span data-ttu-id="f7736-150">tooremove hello távoli asztali bővítmény hello központi telepítéséből, hello használható [Remove-AzureServiceRemoteDesktopExtension](/powershell/module/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="f7736-150">tooremove hello remote desktop extension from hello deployment, you can use hello [Remove-AzureServiceRemoteDesktopExtension](/powershell/module/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet.</span></span> <span data-ttu-id="f7736-151">Opcionálisan kiegészítheti hello üzembe helyezési pont és a szerepkör, amelyből el kívánja tooremove hello távoli asztali bővítmény.</span><span class="sxs-lookup"><span data-stu-id="f7736-151">You can also optionally specify hello deployment slot and role from which you want tooremove hello remote desktop extension.</span></span>

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

> [!NOTE]
> <span data-ttu-id="f7736-152">toocompletely eltávolítása hello bővítménykonfiguráció, meg kell hívni hello *eltávolítása* hello parancsmagot **UninstallConfiguration** paraméter.</span><span class="sxs-lookup"><span data-stu-id="f7736-152">toocompletely remove hello extension configuration, you should call hello *remove* cmdlet with hello **UninstallConfiguration** parameter.</span></span>
>
> <span data-ttu-id="f7736-153">Hello **UninstallConfiguration** paraméter eltávolítja a bővítmény konfigurációja, amely alkalmazott toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f7736-153">hello **UninstallConfiguration** parameter uninstalls any extension configuration that is applied toohello service.</span></span> <span data-ttu-id="f7736-154">Minden bővítménykonfiguráció konfigurációhoz társítva hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f7736-154">Every extension configuration is associated with hello service configuration.</span></span> <span data-ttu-id="f7736-155">Hívása hello *eltávolítása* nélkül parancsmag **UninstallConfiguration** hello megszünteti <mark>telepítési</mark> hello bővítmény konfigurációjából, így hatékony eltávolítása hello bővítmény.</span><span class="sxs-lookup"><span data-stu-id="f7736-155">Calling hello *remove* cmdlet without **UninstallConfiguration** disassociates hello <mark>deployment</mark> from hello extension configuration, thus effectively removing hello extension.</span></span> <span data-ttu-id="f7736-156">Azonban a hello bővítménykonfiguráció marad hello szolgáltatáshoz tartozik.</span><span class="sxs-lookup"><span data-stu-id="f7736-156">However, hello extension configuration remains associated with hello service.</span></span>
>
>

## <a name="additional-resources"></a><span data-ttu-id="f7736-157">További források</span><span class="sxs-lookup"><span data-stu-id="f7736-157">Additional resources</span></span>

<span data-ttu-id="f7736-158">[Hogyan tooConfigure Felhőszolgáltatások](cloud-services-how-to-configure.md)
[Cloud services – gyakori kérdések – a távoli asztal](cloud-services-faq.md)</span><span class="sxs-lookup"><span data-stu-id="f7736-158">[How tooConfigure Cloud Services](cloud-services-how-to-configure.md)
[Cloud services FAQ - Remote Desktop](cloud-services-faq.md)</span></span>
