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
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>Azure Cloud Services PowerShell használatával a szerepkör távoli asztali kapcsolat engedélyezése
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [klasszikus Azure portál](cloud-services-role-enable-remote-desktop.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)
>
>

A távoli asztal tooaccess hello asztali egy Azure-beli szerepkör lehetővé teszi. A távoli asztali kapcsolat tootroubleshoot használja, és hibáinak az alkalmazás futtatása során.

Ez a cikk ismerteti, hogyan tooenable távoli asztal a PowerShell használatával a Felhőszolgáltatás szerepköreit. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) Ez a cikk szükséges hello előfeltételeinek. PowerShell használja a távoli asztali bővítmény hello, ahol engedélyezheti a távoli asztal, hello alkalmazás telepítése után.

## <a name="configure-remote-desktop-from-powershell"></a>A PowerShell távoli asztal konfigurálása
Hello [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) parancsmag lehetővé teszi a távoli asztal tooenable megadott szerepkörök vagy a cloud service-környezet minden szerepkört. hello parancsmag lehetővé teszi, hogy adja meg az hello távoli asztal felhasználójának keresztül hello hello felhasználónév és jelszó *Credential* paraméter, amely fogad egy PSCredential objektumot.

Ha PowerShell interaktív módon használja, egyszerűen beállíthatja hello PSCredential objektum által hívó hello [Get-hitelesítő adatok](https://technet.microsoft.com/library/hh849815.aspx) parancsmag.

```
$remoteusercredentials = Get-Credential
```

Ez a parancs megjelenít egy párbeszédpanelt, így Ön tooenter hello felhasználónév és a jelszó hello távoli biztonságos módon.

Mivel a PowerShell segítségével az automation-forgatókönyvek, is beállíthat hello **PSCredential** objektum oly módon, hogy nincs szükség felhasználói beavatkozásra. Először tooset biztonságos jelszó. Egy egyszerű szöveges jelszó átalakíthatja a tooa biztonságos karakterlánc megadását előtaggal használatával [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx). Ezután kell tooconvert a biztonságos karakterláncot kell megadnia egy titkosított szabványos karakterláncok használatával történő [ConvertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx). Most a titkosított szabványos karakterlánc tooa fájl használatával mentheti [Set-tartalom](https://technet.microsoft.com/library/ee176959.aspx).

A biztonságos jelszó fájlt, hogy nincs tootype hello jelszó minden alkalommal is létrehozhatja. Biztonságos jelszó fájl is nagyobb, mint egy egyszerű szöveges fájl. A következő PowerShell toocreate biztonságos jelszó fájl hello használata:

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

> [!IMPORTANT]
> Hello jelszó beállításakor győződjön meg arról, hogy teljesülnek-e hello [bonyolult](https://technet.microsoft.com/library/cc786468.aspx).
>
>

toocreate hello hitelesítő objektum hello biztonságos jelszó fájlból kell hello fájl tartalmának olvasása és alakíthatja át őket hátsó tooa biztonságos karakterláncot [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).

Hello [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) parancsmag is fogad el egy *lejárati* paraméter, amely megadja egy **DateTime** hello felhasználói fiók lejár. Például beállíthat hello fiók tooexpire néhány nap múlva az aktuális dátum és idő hello.

A PowerShell-példa bemutatja, hogyan tooset hello egy felhőalapú szolgáltatás a távoli asztali bővítmény:

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
Opcionálisan kiegészítheti hello üzembe helyezési pont és a szerepköröket, amelyek a távoli asztal tooenable szeretné. Ha ezek a paraméterek nincsenek megadva, hello parancsmag lehetővé teszi, hogy minden szerepkörök hello a távoli asztal **éles** üzembe helyezési pont.

Távoli asztali bővítmény hello társítva a központi telepítés. Ha létrehoz egy új hello szolgáltatás központi telepítését, tooenable távoli asztal van központi telepítést. Ha azt szeretné, hogy engedélyezve van a távoli asztal toohave, majd érdemes hello PowerShell-parancsfájlok integrálása a telepítési munkafolyamat.

## <a name="remote-desktop-into-a-role-instance"></a>Távoli asztali kapcsolatot egy szerepkörpéldányt
Hello [Get-AzureRemoteDesktopFile](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) parancsmag használt tooremote asztali kapcsolatot egy adott szerepkör példánya a felhőalapú szolgáltatás. Használhatja a hello *LocalPath* paraméter toodownload hello RDP helyileg a fájlt. Vagy használhat hello *indítása* paraméter toodirectly indítási hello távoli asztali kapcsolat párbeszédpanel tooaccess hello felhőalapú szolgáltatás szerepkör példánya.

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>Ellenőrizze, hogy engedélyezve van-e a távoli asztal bővítmény egy szolgáltatásra
Hello [Get-AzureServiceRemoteDesktopExtension](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) parancsmag jeleníti meg, hogy a távoli asztal engedélyezve van-e a szolgáltatás üzemelő példányon. hello parancsmag hello távoli asztal felhasználójának és a távoli asztali bővítmény hello engedélyezett hello szerepkörök hello felhasználónevét adja vissza. Alapértelmezés szerint ez hello üzembe helyezési pont történik, és kiválaszthatja a tárolóhely ehelyett átmeneti toouse hello.

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>A szolgáltatás a távoli asztal-kiterjesztés eltávolítása
Ha hello távoli asztali bővítmény üzemelő példányon már engedélyezve van, és kell tooupdate hello távoli asztal beállításait, először távolítsa el a hello bővítmény. Majd engedélyezze újra az új beállítások hello. Például ha azt szeretné, hogy tooset hello távoli felhasználói fiókot, vagy hello fiókot új jelszó lejárt. Ezzel a meglévő központi telepítésben hello távoli asztali engedélyezett bővítményekhez szükséges. Az új központi telepítéseknél egyszerűen alkalmazhat hello bővítmény közvetlenül.

tooremove hello távoli asztali bővítmény hello központi telepítéséből, hello használható [Remove-AzureServiceRemoteDesktopExtension](/powershell/module/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) parancsmag. Opcionálisan kiegészítheti hello üzembe helyezési pont és a szerepkör, amelyből el kívánja tooremove hello távoli asztali bővítmény.

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

> [!NOTE]
> toocompletely eltávolítása hello bővítménykonfiguráció, meg kell hívni hello *eltávolítása* hello parancsmagot **UninstallConfiguration** paraméter.
>
> Hello **UninstallConfiguration** paraméter eltávolítja a bővítmény konfigurációja, amely alkalmazott toohello szolgáltatás. Minden bővítménykonfiguráció konfigurációhoz társítva hello szolgáltatást. Hívása hello *eltávolítása* nélkül parancsmag **UninstallConfiguration** hello megszünteti <mark>telepítési</mark> hello bővítmény konfigurációjából, így hatékony eltávolítása hello bővítmény. Azonban a hello bővítménykonfiguráció marad hello szolgáltatáshoz tartozik.
>
>

## <a name="additional-resources"></a>További források

[Hogyan tooConfigure Felhőszolgáltatások](cloud-services-how-to-configure.md)
[Cloud services – gyakori kérdések – a távoli asztal](cloud-services-faq.md)
