---
title: "Azure Cloud Services szerepkörök – gyakori kérdések |} Microsoft Docs"
description: "Azure Cloud Services vonatkozó gyakran ismételt kérdések. Tanúsítványok, a webes szerepkörök és a feldolgozói szerepkörök kapcsolatos gyakori kérdésekre ad választ."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: d887f3b31693c414254dc01dac4dbdd6d9224b6d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="cloud-services-faq"></a>Cloud Services – gyakori kérdések
Ebben a cikkben megválaszolunk néhány gyakori kérdés a Microsoft Azure felhőalapú szolgáltatásairól. Is letöltheti a [Azure támogatási gyakori Kérdéseinek](http://go.microsoft.com/fwlink/?LinkID=185083) általános Azure tarifa- és támogatási információkat. Is tud kezelni a [felhőalapú szolgáltatások Virtuálisgép-méretet lap](cloud-services-sizes-specs.md) mérete információt.

## <a name="certificates"></a>Tanúsítványok
### <a name="where-should-i-install-my-certificate"></a>Hol kell telepíteni a tanúsítványt?
* **A**  
  Alkalmazás titkos kulcsot tartalmazó tanúsítvány (\*.pfx, \*.p12).
* **HITELESÍTÉSSZOLGÁLTATÓ**  
  Ezt a tárolót (házirend- és alárendelt hitelesítésszolgáltatókat) keresse meg a köztes tanúsítványok.
* **LEGFELSŐ SZINTŰ**  
  A legfelső szintű hitelesítésszolgáltató tárolja, ezért itt kell nyissa meg a fő legfelső szintű Hitelesítésszolgáltatói tanúsítvány.

### <a name="i-cant-remove-expired-certificate"></a>A lejárt tanúsítvány nem távolítható el
Azure megakadályozza a tanúsítvány eltávolítása, miközben használatban van. Vagy törölje a központi telepítést, a tanúsítványt használó, vagy frissítse a központi telepítés egy másik vagy megújított tanúsítványt kell.

### <a name="delete-an-expired-certificate"></a>A lejárt tanúsítvány törlése
Mindaddig, amíg a tanúsítvány nincs használatban, használhatja a [Remove-AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) PowerShell-parancsmag segítségével távolítsa el a tanúsítványt.

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a>I lejárt tanúsítványok nevű Windows Azure Szolgáltatáskezelés-bővítmények
Ezek a tanúsítványok jönnek létre, amikor egy bővítmény hozzáadódik a felhőszolgáltatásra, például a távoli asztali bővítmény. Ezek a tanúsítványok csak titkosítása és visszafejtése a bővítmény privát konfigurációjának használják. Nem számít, ha ezek a tanúsítványok lejárnak. A lejárati dátum a rendszer nem ellenőrzi.

### <a name="certificates-i-have-deleted-keep-reappearing"></a>Törölt rendelkezik tanúsítványok továbbra is megjelennek
Ezek továbbra is megjelennek a legvalószínűbb ok miatt használ, például a Visual Studio eszközt. Amikor újra kapcsolódik egy eszközt, amely olyan tanúsítványt használ, azt újra feltöltés Azure.

### <a name="my-certificates-keep-disappearing"></a>A tanúsítványok ne hagyja tűnhessenek
A virtuálisgép-példányt újraindul, az összes helyi módosítások elvesznek. Használja a [indítási tevékenységhez](cloud-services-startup-tasks.md) tanúsítványok telepítése a virtuális géphez a szerepkör minden egyes indításakor.

### <a name="i-cannot-find-my-management-certificates-in-the-portal"></a>Nem található a felügyeleti tanúsítványok a portálon
[Felügyeleti tanúsítványok](../azure-api-management-certs.md) csak érhetők el a klasszikus Azure-portálon. Azure-portálon nem használja a felügyeleti tanúsítványok. 

### <a name="how-can-i-disable-a-management-certificate"></a>Hogyan letilthatja egy felügyeleti tanúsítványt?
[Felügyeleti tanúsítványok](../azure-api-management-certs.md) nem tiltható le. Törli a klasszikus Azure portálon keresztül, ha azt szeretné, hogy már használható.

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a>Hogyan hozható létre egy adott IP-cím SSL-tanúsítvány?
Kövesse a riasztásban megjelenő utasításokat a [hozzon létre egy tanúsítványt az oktatóanyag](cloud-services-certs-create.md). Az IP-címet használják a DNS-nevét.

## <a name="security"></a>Biztonság
### <a name="disable-ssl-30"></a>Tiltsa le a SSL 3.0
Tiltsa le a SSL 3.0 és TLS biztonsági használatához ebben a blogbejegyzésben amely dokumentált indítási feladat létrehozása: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/

### <a name="add-nosniff-to-your-website"></a>Adja hozzá **nosniff** a webhelyhez
Megakadályozza a felhasználókat a MIME-típusok elemzés, adja hozzá a megfelelő értéket a *web.config* fájlt.

```xml
<configuration>
   <system.webServer>
      <httpProtocol>
         <customHeaders>
            <add name="X-Content-Type-Options" value="nosniff" />
         </customHeaders>
      </httpProtocol>
   </system.webServer>
</configuration>
```

Azt is megteheti ezt az IIS-beállításként. Használja a következő parancsot a [közös indítási feladatok](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) cikk.

```cmd
%windir%\system32\inetsrv\appcmd set config /section:httpProtocol /+customHeaders.[name='X-Content-Type-Options',value='nosniff']
```

### <a name="customize-iis-for-a-web-role"></a>A webes szerepkör IIS testreszabása
Az IIS indítási parancsfájl használatát a [közös indítási feladatok](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) cikk.

## <a name="scaling"></a>Méretezés
### <a name="i-cannot-scale-beyond-x-instances"></a>I nem felüli X példányok
Az Azure-előfizetés használhatja magok száma van korlátozva. Skálázás nem működik, ha az összes rendelkezésre álló magot használja. Például ha egy vonatkozó felső korlátját: 100, ez azt jelenti, 100 A1 méretű virtuálisgép-példánya lehet a felhőalapú szolgáltatás, vagy 50 A2 méretű virtuálisgép-példánya.

## <a name="networking"></a>Hálózat
### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>I multi-VIP felhőszolgáltatásban olyan IP-címet nem sikerült lefoglalni
Először is győződjön meg arról, hogy a virtuálisgép-példányt, amely az IP-Címek lefoglalása próbál be van-e kapcsolva. Második győződjön meg arról, hogy használata fenntartott IP-címek többet jelzi az átmeneti és üzemi központi telepítéseket. **Ne** módosíthatja a beállításokat, amíg a telepítés frissítését végzi.

## <a name="remote-desktop"></a>A távoli asztal
### <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a>Hogyan készíthetek távoli asztal Ha egy NSG-t?
Szabályok hozzáadása az NSG-t, amelyek lehetővé teszik a forgalom a portokon **3389-es** és **20000**.  A távoli asztal-portot használja **3389-es**.  Felhőalapú szolgáltatás példányainak kerül, így nem közvetlenül szabályozhatja, hogy melyik példányt való csatlakozáshoz.  A *RemoteForwarder* és *RemoteAccess* ügynökök kezelése RDP-forgalmát, és az ügyfél küldése az RDP cookie, és adjon meg egy egyedi példány való csatlakozáshoz.  A *RemoteForwarder* és *RemoteAccess* ügynökök szükséges, hogy a port **20000*** nyitható meg, amely blokkolhatja, ha egy NSG.
