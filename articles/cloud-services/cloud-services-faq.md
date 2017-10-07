---
title: "aaaAzure Felhőszolgáltatások szerepkörök – gyakori kérdések |} Microsoft Docs"
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
ms.openlocfilehash: b07a1990e031e60ae919a5f7c636945b89c7d3a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-services-faq"></a>Cloud Services – gyakori kérdések
Ebben a cikkben megválaszolunk néhány gyakori kérdés a Microsoft Azure felhőalapú szolgáltatásairól. Meglátogathatja hello [Azure támogatási gyakori Kérdéseinek](http://go.microsoft.com/fwlink/?LinkID=185083) általános Azure tarifa- és támogatási információkat. Is további részleteket hello [felhőalapú szolgáltatások Virtuálisgép-méretet lap](cloud-services-sizes-specs.md) mérete információt.

## <a name="certificates"></a>Tanúsítványok
### <a name="where-should-i-install-my-certificate"></a>Hol kell telepíteni a tanúsítványt?
* **A**  
  Alkalmazás titkos kulcsot tartalmazó tanúsítvány (\*.pfx, \*.p12).
* **HITELESÍTÉSSZOLGÁLTATÓ**  
  Ezt a tárolót (házirend- és alárendelt hitelesítésszolgáltatókat) keresse meg a köztes tanúsítványok.
* **LEGFELSŐ SZINTŰ**  
  hello legfelső szintű hitelesítésszolgáltató tárolásához, ezért itt kell nyissa meg a fő legfelső szintű Hitelesítésszolgáltatói tanúsítvány.

### <a name="i-cant-remove-expired-certificate"></a>A lejárt tanúsítvány nem távolítható el
Azure megakadályozza a tanúsítvány eltávolítása, miközben használatban van. Tooeither delete hello telepítés hello tanúsítványt használ, vagy különböző vagy megújított tanúsítványt hello központi telepítés szükséges.

### <a name="delete-an-expired-certificate"></a>A lejárt tanúsítvány törlése
Mindaddig, amíg hello tanúsítvány nincs használatban, használhatja a hello [Remove-AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) PowerShell parancsmag tooremove egy tanúsítványt.

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a>I lejárt tanúsítványok nevű Windows Azure Szolgáltatáskezelés-bővítmények
Ezeket a tanúsítványokat jönnek létre, amikor egy bővítmény toohello felhőszolgáltatásra, például a távoli asztali bővítmény hello kerül. Ezek a tanúsítványok csak titkosítása és visszafejtése hello a hello bővítmény privát konfigurációjának használják. Nem számít, ha ezek a tanúsítványok lejárnak. hello lejárati dátum a rendszer nem ellenőrzi.

### <a name="certificates-i-have-deleted-keep-reappearing"></a>Törölt rendelkezik tanúsítványok továbbra is megjelennek
Ezek továbbra is megjelennek a legvalószínűbb ok miatt használ, például a Visual Studio eszközt. Csatlakozzon újra az olyan eszköz, amely olyan tanúsítványt használ, amikor újra lesz feltöltött tooAzure.

### <a name="my-certificates-keep-disappearing"></a>A tanúsítványok ne hagyja tűnhessenek
Amikor virtuálisgép-példányt hello újraindul, az összes helyi módosítások elvesznek. Használja a [indítási tevékenységhez](cloud-services-startup-tasks.md) tooinstall tanúsítványok toohello virtuális gép minden alkalommal hello szerepkör elindul.

### <a name="i-cannot-find-my-management-certificates-in-hello-portal"></a>Nem található a felügyeleti tanúsítványok hello portálon
[Felügyeleti tanúsítványok](../azure-api-management-certs.md) csak találhatók hello klasszikus Azure portálon. hello Azure-portálon nem használja a felügyeleti tanúsítványok. 

### <a name="how-can-i-disable-a-management-certificate"></a>Hogyan letilthatja egy felügyeleti tanúsítványt?
[Felügyeleti tanúsítványok](../azure-api-management-certs.md) nem tiltható le. Törli őket hello klasszikus Azure portálon keresztül, ha nem szeretné, hogy azok többé használt toobe.

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a>Hogyan hozható létre egy adott IP-cím SSL-tanúsítvány?
Hello hello útmutatásait követve [hozzon létre egy tanúsítványt az oktatóanyag](cloud-services-certs-create.md). DNS-név hello hello IP-címet használja.

## <a name="security"></a>Biztonság
### <a name="disable-ssl-30"></a>Tiltsa le a SSL 3.0
toodisable SSL 3.0 és TLS biztonság használata, amely dokumentálja indítási feladat létrehozása az ebben a blogbejegyzésben: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/

### <a name="add-nosniff-tooyour-website"></a>Adja hozzá **nosniff** tooyour webhely
elemzés hello MIME-típusok, az ügynökök tooprevent adja hozzá a megfelelő értéket a *web.config* fájlt.

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

Azt is megteheti ezt az IIS-beállításként. Használjon hello következő parancsot a hello [közös indítási feladatok](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) cikk.

```cmd
%windir%\system32\inetsrv\appcmd set config /section:httpProtocol /+customHeaders.[name='X-Content-Type-Options',value='nosniff']
```

### <a name="customize-iis-for-a-web-role"></a>A webes szerepkör IIS testreszabása
Hello IIS indítási parancsfájl hello használatát [közös indítási feladatok](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) cikk.

## <a name="scaling"></a>Méretezés
### <a name="i-cannot-scale-beyond-x-instances"></a>I nem felüli X példányok
Az Azure-előfizetés használhatja magok hello számára van korlátozva. Skálázás nem működik, ha elérhető hello magot használja. Például ha egy vonatkozó felső korlátját: 100, ez azt jelenti, 100 A1 méretű virtuálisgép-példánya lehet a felhőalapú szolgáltatás, vagy 50 A2 méretű virtuálisgép-példánya.

## <a name="networking"></a>Hálózat
### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>I multi-VIP felhőszolgáltatásban olyan IP-címet nem sikerült lefoglalni
Először is győződjön meg arról, hogy hello virtuálisgép-példányt próbált tooreserve hello IP-Címek be van kapcsolva. Ezután ellenőrizze, hogy hello átmeneti és üzemi telepítések többet jelzi a foglalt IP-cím használja. **Ne** hello beállításainak módosítása közben hello telepítési frissítését végzi.

## <a name="remote-desktop"></a>A távoli asztal
### <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a>Hogyan készíthetek távoli asztal Ha egy NSG-t?
Adja hozzá a szabályok toohello, amelyek lehetővé teszik a forgalom a portokon NSG **3389-es** és **20000**.  A távoli asztal-portot használja **3389-es**.  Felhőalapú szolgáltatás példányainak kerül, így nem közvetlenül szabályozhatja, hogy melyik példányt tooconnect számára.  Hello *RemoteForwarder* és *RemoteAccess* ügynökök RDP-forgalom kezelésére és hello ügyfél toosend RDP cookie-k engedélyezése, és adjon meg egy egyedi példány tooconnect való.  Hello *RemoteForwarder* és *RemoteAccess* ügynökök szükséges, hogy a port **20000*** nyitható meg, amely blokkolhatja, ha egy NSG.
