---
title: "Konfigurációs és kezelésének számos, a Microsoft Azure Cloud Services – gyakori kérdések |} Microsoft Docs"
description: "Ez a cikk a kapcsolatos gyakori kérdések konfigurálása és kezelése a Microsoft Azure Felhőszolgáltatások sorolja fel."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: 42b5d2947df92b4486fe149d046168208083dde2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="configuration-and-management-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Konfigurációs és kezelésének számos Azure-szolgáltatásokhoz: gyakran ismételt kérdések (GYIK)

Ez a cikk tartalmazza a konfigurációs és kezelésének számos kapcsolatos gyakran ismételt kérdések [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Is tud kezelni a [felhőalapú szolgáltatások Virtuálisgép-méretet lap](cloud-services-sizes-specs.md) mérete információt.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="how-do-i-add-nosniff-to-my-website"></a>Hogyan "nosniff" hozzáadása a webhely?
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

## <a name="how-do-i-customize-iis-for-a-web-role"></a>Hogyan testre az IIS egy webes szerepkör?
Az IIS indítási parancsfájl használatát a [közös indítási feladatok](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) cikk.

## <a name="i-cannot-scale-beyond-x-instances"></a>I nem felüli X példányok
Az Azure-előfizetés használhatja magok száma van korlátozva. Skálázás nem működik, ha az összes rendelkezésre álló magot használja. Például ha egy vonatkozó felső korlátját: 100, ez azt jelenti, 100 A1 méretű virtuálisgép-példánya lehet a felhőalapú szolgáltatás, vagy 50 A2 méretű virtuálisgép-példánya.

## <a name="how-can-i-implement-role-based-access-for-cloud-services"></a>Hogyan is létrehozható a Felhőszolgáltatások szerepkörön alapuló hozzáférés?
Cloud Services nem támogatja a szerepköralapú hozzáférés-vezérlést (RBAC) modell a, mert nincs olyan Azure Resource Manager-alapú szolgáltatás.

Lásd: [hagyományos előfizetés rendszergazdái és az Azure RBAC](../active-directory/role-based-access-control-what-is.md#azure-rbac-vs-classic-subscription-administrators).

## <a name="how-do-i-set-the-idle-timeout-for-azure-load-balancer"></a>Hogyan állíthatom üresjárati időkorlátját az Azure terheléselosztó?
Az időtúllépés adhat meg a szolgáltatás definíciós (csdef) fájl ehhez hasonló:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="mgVS2015Worker" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10100"   idleTimeoutInMinutes="30" />
    </Endpoints>
  </WorkerRole>
```
Lásd: [új: Azure Load Balancer konfigurálható az üresjárati időkorlátjának](https://azure.microsoft.com/blog/new-configurable-idle-timeout-for-azure-load-balancer/) további információt.

## <a name="can-microsoft-internal-engineers-rdp-to-cloud-service-instances-without-permission"></a>A Microsoft belső szakemberei RDP cloud service példányokhoz engedélye nélkül is?
A Microsoft egy szigorú folyamat, amely a felhőszolgáltatás (e-mailben vagy más írásbeli kommunikációs) írásbeli engedélye nélkül történő távoli asztali belső mérnökök nem teszi lehetővé a tulajdonos vagy az általa kijelölt szervet követi.

## <a name="why-is-the-certificate-chain-of-my-cloud-service-ssl-certificate-incomplete"></a>Miért nem fejeződött be a tanúsítványlánc a felhőalapú szolgáltatás SSL-tanúsítvány?
Azt javasoljuk, hogy a teljes tanúsítványlánc (a levél cert, a köztes tanúsítványok és a legfelső szintű tanúsítvány) helyett a levéltanúsítvány telepítését. A levéltanúsítvány telepítésekor támaszkodnak Windows érdekében a megbízható tanúsítványok Listáját a tanúsítványlánc létrehozásához. Ha a hálózati vagy DNS-probléma történt jelentkezik az Azure vagy a Windows Update a Windows megpróbálja érvényesíteni a tanúsítványt, akkor a tanúsítvány érvénytelen tekinthetők. A teljes tanúsítványlánc telepítése, a probléma elkerülhető. A következő blog [láncolt SSL-tanúsítvány telepítése](https://blogs.msdn.microsoft.com/azuredevsupport/2010/02/24/how-to-install-a-chained-ssl-certificate/) bemutatja, hogyan ehhez.

## <a name="how-do-i-associate-a-static-ip-address-to-my-cloud-service"></a>Hogyan rendelheti hozzá egy statikus IP-címet az a cloud service?
Egy statikus IP-cím beállításához létrehozásához szükséges egy fenntartott IP-cím. A foglalt IP-cím is lehet társítani, egy új felhőalapú szolgáltatás, vagy egy meglévő telepítését. Tekintse meg a részleteket a következő dokumentumokat:
* [A fenntartott IP-cím létrehozása](../virtual-network/virtual-networks-reserved-public-ip.md#manage-reserved-vips)
* [Foglaljon le egy meglévő felhőszolgáltatáshoz IP-címe](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
* [Új felhőalapú szolgáltatás foglalt IP-cím hozzárendelése](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-new-cloud-service)
* [Egy futó telepítéshez foglalt IP-cím hozzárendelése](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-running-deployment)
* [A foglalt IP-cím egy felhőszolgáltatásra történő társíthatók a szolgáltatás konfigurációs fájl használatával](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file)

## <a name="what-is-the-quota-limit-for-my-cloud-service"></a>Mi az, hogy a kvótakorlát miatt a felhőalapú szolgáltatáshoz?
Lásd: [szolgáltatásspecifikus korlátozza](../azure-subscription-service-limits.md#subscription-limits).

## <a name="why-does-the-drive-on-my-cloud-service-vm-show-very-little-free-disk-space"></a>Miért nem jelennek meg a meghajtót, a felhőalapú szolgáltatás VM nagyon kevés a szabad lemezterület?
Ez az elvárt működés, és ennek nem szabad következtében az alkalmazásba bármilyen probléma. Naplózási be van kapcsolva a % uproot % meghajtót az Azure PaaS virtuális gépeken, amelyek alapvetően használ a fájlok általában foglalnak lemezterületről összeg fel. Azonban számos szempontot figyelembe vennie, amely lényegében a ikonná ez nem probléma.

% Approot % meghajtó méretének kiszámítása: < méret .cspkg + napló maximális mérete > + os szabad lemezterület, vagy 1,5 GB-os, amelyik történik, nagyobb. A virtuális gép mérete nincs hatással a számítás rendelkezik. (A Virtuálisgép-méretet csak hatással van az ideiglenes C: meghajtóra.) 

A % approot % meghajtóra írása nem támogatott. Ha az Azure virtuális gépen, Önnek kell megtennie ideiglenes LocalStorage az erőforrás (vagy a másik lehetőség, például a Blob-tároló, Azure-fájlok, stb.). Így mappa % approot % szabad terület mennyisége értéke nem értelmezhető. Ha nem biztos benne, hogy az alkalmazás a % approot % meghajtóra ír, néhány nap múlva futtatása, és hasonlítsa össze a szolgáltatás mindig megőrizheti a "előtt" és "utána" méretét. 

Azure nem fog kiírni, semmit a % approot % meghajtóra. Amikor a virtuális Merevlemezt a .cspkg alapján létrehozott, és az Azure virtuális Géphez csatlakoztatva, előfordulhat, hogy a meghajtó írni egyedül az alkalmazás. 

A napló profilbeállításai nem konfigurálható, ezért nem kapcsolható ki azt.

## <a name="what-are-the-features-and-capabilities-that-azure-basic-ipsids-and-ddos-provides"></a>Mik a szolgáltatásokat és képességeket biztosító Azure alapvető IP-CÍMEK/Azonosítók és DDOS?
Azure datacenter fizikai kiszolgálók fenyegetések elleni védelemre IP-CÍMEK/Azonosítók van. Ezenkívül az ügyfelek külső biztonsági megoldások, például a webalkalmazási tűzfalak, hálózati tűzfalak, kártevőirtó, behatolásérzékelési, megelőzési rendszerek (Azonosítók/IP-CÍMEK), és több telepíthetnek. További információkért lásd: [az adatok és eszközök védelme és a globális biztonsági szabványoknak való megfelelés](https://www.microsoft.com/en-us/trustcenter/Security/AzureSecurity).

Microsoft folyamatosan figyeli a kiszolgálók, hálózatok és alkalmazások észleli a veszélyeket. Azure multipronged fenyegetés-felügyeleti megközelítést használ behatolásérzékelési, elosztott-szolgáltatásmegtagadásos (DDoS-) támadások megelőzése, tesztelési, viselkedési analytics behatolást vagy a biztonság, anomáliadetektálás és gépi tanulási folyamatosan megerősítése a védelmet, és a kockázatok csökkentése. Az Azure-hoz a Microsoft Antimalware védelmet nyújt az Azure felhőszolgáltatások és virtuális gépek. Lehetősége van harmadik fél biztonsági megoldások emellett, például a webes alkalmazás tűz falakat emelnek, hálózati tűzfalak, kártevőirtó, behatolás felderítésére és megelőzésére rendszerek (Azonosítók/IP-CÍMEK), és több központi telepítéséhez.

## <a name="why-does-iis-stop-writing-to-the-log-directory"></a>Miért nem ne a naplózási könyvtár írjanak IIS?
A helyi tárhelykvótát a naplózási könyvtár írásra kimerített. Ennek elkerülése érdekében tegye három lehetőség közül:
* Lehetővé teszi az IIS és a diagnosztika rendszeresen került át blob-tároló.
* Naplófájlok manuálisan távolítsa el a naplózási könyvtárat.
* Növelje a helyi erőforrások kvótáját.

További információkért lásd a következő dokumentumokat:
* [Diagnosztikai adatok tárolása és megtekintése az Azure Storage-ban](cloud-services-dotnet-diagnostics-storage.md)
* [IIS-napló leállítja a felhőszolgáltatást írt](https://blogs.msdn.microsoft.com/cie/2013/12/21/iis-logs-stops-writing-in-cloud-service/)

## <a name="what-is-the-purpose-of-the-windows-azure-tools-encryption-certificate-for-extensions"></a>Mi az a célja a "Windows Azure eszközök titkosítási tanúsítványt a Extensions"?
Ezeket a tanúsítványokat automatikusan jönnek létre, amikor egy bővítmény hozzáadódik a felhőalapú szolgáltatás. Leggyakrabban Ez a ÜVEGVATTA kiterjesztés vagy a RDP-bővítményt, de annak oka az lehet, például a kártevőirtó vagy naplógyűjtő bővítmény. Ezek a tanúsítványok csak titkosítása és visszafejtése a bővítmény privát konfigurációjának használják. A lejárati dátum soha nem ellenőrzi, tehát nem számít, ha a tanúsítvány lejárt. 

Ezek a tanúsítványok figyelmen kívül hagyhatja. Ha azt szeretné, a tanúsítványok karbantartása, próbálja meg őket az összes törlése. Azure hibaüzenetet küldjön, ha törli a tanúsítványt, amely használatban van.

## <a name="how-can-i-generate-a-certificate-signing-request-csr-without-rdp-ing-in-to-the-instance"></a>Hogyan tudom generálhatnak egy tanúsítvány-aláírási kérelem (CSR) nélkül "RDP-ing" példányához?
Tekintse meg a következő útmutató:

>[Megszerezni a tanúsítványt a Windows Azure-webhelyek (WAWS)](https://azure.microsoft.com/blog/obtaining-a-certificate-for-use-with-windows-azure-web-sites-waws/)

Vegye figyelembe, hogy a CSR-e egy szövegfájlt. Nincs a számítógépről a létrehozandó amelyeken a tanúsítvány végső soron kell használni. Bár ez a dokumentum írása egy App Service, a CSR létrehozása általános, és a Felhőszolgáltatások is vonatkozik.
