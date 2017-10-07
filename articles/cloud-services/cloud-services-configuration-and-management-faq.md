---
title: "a Microsoft Azure Cloud Services – gyakori kérdések aaaConfiguration és kezelésének számos |} Microsoft Docs"
description: "Ez a cikk felsorolja hello konfigurálása és kezelése a Microsoft Azure Felhőszolgáltatások vonatkozó gyakran ismételt kérdések."
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
ms.openlocfilehash: 62ece142ac0ef5d45081cab333375b1a0a8f0ab7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuration-and-management-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Konfigurációs és kezelésének számos Azure-szolgáltatásokhoz: gyakran ismételt kérdések (GYIK)

Ez a cikk tartalmazza a konfigurációs és kezelésének számos kapcsolatos gyakran ismételt kérdések [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Is további részleteket hello [felhőalapú szolgáltatások Virtuálisgép-méretet lap](cloud-services-sizes-specs.md) mérete információt.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="how-do-i-add-nosniff-toomy-website"></a>Hogyan adja hozzá a "nosniff" toomy webhely?
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

## <a name="how-do-i-customize-iis-for-a-web-role"></a>Hogyan testre az IIS egy webes szerepkör?
Hello IIS indítási parancsfájl hello használatát [közös indítási feladatok](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) cikk.

## <a name="i-cannot-scale-beyond-x-instances"></a>I nem felüli X példányok
Az Azure-előfizetés használhatja magok hello számára van korlátozva. Skálázás nem működik, ha elérhető hello magot használja. Például ha egy vonatkozó felső korlátját: 100, ez azt jelenti, 100 A1 méretű virtuálisgép-példánya lehet a felhőalapú szolgáltatás, vagy 50 A2 méretű virtuálisgép-példánya.

## <a name="how-can-i-implement-role-based-access-for-cloud-services"></a>Hogyan is létrehozható a Felhőszolgáltatások szerepkörön alapuló hozzáférés?
Cloud Services nem támogatja a hello szerepköralapú hozzáférés-vezérlést (RBAC) modell, mert nincs olyan Azure Resource Manager-alapú szolgáltatás.

Lásd: [hagyományos előfizetés rendszergazdái és az Azure RBAC](../active-directory/role-based-access-control-what-is.md#azure-rbac-vs-classic-subscription-administrators).

## <a name="how-do-i-set-hello-idle-timeout-for-azure-load-balancer"></a>Hogyan állíthatom hello üresjárati időkorlátját az Azure terheléselosztó?
A szolgáltatás definíciós (csdef) fájl ilyen hello időtúllépés adhat meg:

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

## <a name="can-microsoft-internal-engineers-rdp-toocloud-service-instances-without-permission"></a>A Microsoft belső szakemberei RDP toocloud szolgáltatáspéldány engedélye nélkül is?
Egy szigorú folyamat, amely nem engedélyezi, hogy belső tooRDP szakemberek a felhőalapú szolgáltatás nélkül a Microsoft következő hello tulajdonosa vagy az általa kijelölt szervet írásbeli engedélyével (e-mailben vagy más írásbeli kommunikációs).

## <a name="why-is-hello-certificate-chain-of-my-cloud-service-ssl-certificate-incomplete"></a>Miért nem teljes tanúsítványlánc hello a felhőalapú szolgáltatás SSL-tanúsítvány?
Azt javasoljuk, hogy telepítsék a hello teljes láncát (a levél cert, a köztes tanúsítványok és a legfelső szintű tanúsítvány) csak a hello levéltanúsítvány helyett. Csak a hello levéltanúsítvány telepítésekor használ, toobuild hello tanúsítványlánc Windows hello CTL érdekében. Ha hálózati vagy DNS-probléma történt az Azure vagy a Windows Update Windows toovalidate hello tanúsítványt próbál, hello tanúsítvány érvénytelen tekinthetők. Hello teljes tanúsítványlánc telepítése, a probléma elkerülhető. blog: hello [hogyan tooinstall láncolt SSL-tanúsítvány](https://blogs.msdn.microsoft.com/azuredevsupport/2010/02/24/how-to-install-a-chained-ssl-certificate/) bemutatja, hogyan toodo ez.

## <a name="how-do-i-associate-a-static-ip-address-toomy-cloud-service"></a>Hogyan társítsa a egy statikus IP-cím toomy felhőalapú szolgáltatás?
tooset be egy statikus IP-címet, toocreate egy fenntartott IP-cím szükséges. A foglalt IP-címhez társított tooa új felhőalapú szolgáltatás, vagy tooan meglévő központi telepítési lehet. Tekintse meg a következő részleteket dokumentumok hello:
* [Hogyan toocreate egy fenntartott IP-cím](../virtual-network/virtual-networks-reserved-public-ip.md#manage-reserved-vips)
* [Foglaljon le egy meglévő felhőszolgáltatáshoz hello IP-címe](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
* [A fenntartott IP-tooa új felhőalapú szolgáltatás társítása](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-new-cloud-service)
* [A fenntartott IP-tooa-telepítést futtató társítása](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-running-deployment)
* [Társítson egy fenntartott IP-tooa felhőalapú szolgáltatás a szolgáltatás konfigurációs fájl segítségével](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file)

## <a name="what-is-hello-quota-limit-for-my-cloud-service"></a>Mi az a hello kvótát a felhőalapú szolgáltatáshoz?
Lásd: [szolgáltatásspecifikus korlátozza](../azure-subscription-service-limits.md#subscription-limits).

## <a name="why-does-hello-drive-on-my-cloud-service-vm-show-very-little-free-disk-space"></a>Miért nem jelennek meg a felhőalapú szolgáltatás VM hello meghajtóján nagyon kevés a szabad lemezterület?
Ez az elvárt működés, és azt nem okoz bármilyen probléma tooyour alkalmazás. Naplózási be van kapcsolva a hello % uproot % meghajtót az Azure PaaS virtuális gépeken, amelyek alapvetően használ fel, amely fájlok általában terület kettős hello mennyiségét. Azonban számos szempontot toobe tudomást, amely lényegében kapcsolja be ezt a nem probléma.

hello % approot % meghajtó méret kiszámítása: < méret .cspkg + napló maximális mérete > + os szabad lemezterület, vagy 1,5 GB-os, amelyik történik, nagyobb. a virtuális gép hello mérete nincs hatással a számítás rendelkezik. (Virtuálisgép-méretet hello csak befolyásolja hello ideiglenes C: meghajtó hello méretét.) 

Nem támogatott toowrite toohello % approot % meghajtó. Ha toohello Azure virtuális Gépen, Önnek kell megtennie ideiglenes LocalStorage az erőforrás (vagy a másik lehetőség, például a Blob-tároló, Azure-fájlok, stb.). Ezért hello hello % approot % mappában lévő szabad terület mérete nem értelmezhető. Ha nem biztos benne, hogy az alkalmazás toohello % approot % meghajtóra ír, mindig megőrizheti a szolgáltatás néhány nap múlva futtatása, és hasonlítsa össze hello "előtt" és "utána" méretét. 

Azure nem fog kiírni, semmit toohello % approot % meghajtót. Amikor hello VHD-t a .cspkg alapján létrehozott, és hello Azure virtuális gép csatlakoztatva, előfordulhat, hogy írást toothis meghajtó hello egyedül az alkalmazás. 

hello napló beállítások esetén nem konfigurálható, ezért nem kapcsolható ki azt.

## <a name="what-are-hello-features-and-capabilities-that-azure-basic-ipsids-and-ddos-provides"></a>Mik azok a hello szolgáltatásokat és képességeket biztosító Azure alapvető IP-CÍMEK/Azonosítók és DDOS?
Azure rendelkezik IP-CÍMEK/Azonosítók az Adatközpont fizikai kiszolgálók toodefend fenyegetésekkel szemben. Ezenkívül az ügyfelek külső biztonsági megoldások, például a webalkalmazási tűzfalak, hálózati tűzfalak, kártevőirtó, behatolásérzékelési, megelőzési rendszerek (Azonosítók/IP-CÍMEK), és több telepíthetnek. További információkért lásd: [az adatok és eszközök védelme és a globális biztonsági szabványoknak való megfelelés](https://www.microsoft.com/en-us/trustcenter/Security/AzureSecurity).

Microsoft folyamatosan figyeli a kiszolgálók, hálózatok és alkalmazások toodetect fenyegetéseket. Azure multipronged fenyegetés-felügyeleti megközelítést használ a behatolás-észlelés, elosztott--szolgáltatásmegtagadásos (DDoS-) támadás megelőzése, behatolást vagy a biztonság tesztelése, viselkedéselemzés, anomáliadetektálás, és a gépi tanulás tooconstantly a védelmi megerősítése és a kockázatok csökkentése. Az Azure-hoz a Microsoft Antimalware védelmet nyújt az Azure felhőszolgáltatások és virtuális gépek. Lehetősége van hello beállítás toodeploy külső biztonsági megoldások emellett, például a webes alkalmazás tűz falakat emelnek, hálózati tűzfalak, kártevőirtó, behatolás felderítésére és megelőzésére rendszerek (Azonosítók/IP-CÍMEK), és több.

## <a name="why-does-iis-stop-writing-toohello-log-directory"></a>Miért nem IIS leállítási toohello naplókönyvtár írása?
Hello helyi tárhelykvótát toohello naplókönyvtár írásra kimerített. Ennek elkerülése érdekében tegye három lehetőség közül:
* Diagnosztika engedélyezése az IIS-hez, és hello diagnosztika rendszeresen áthelyezett tooblob tárolási.
* Manuálisan távolítsa el naplófájlok hello naplózási könyvtárat.
* Növelje a helyi erőforrások kvótáját.

További információkért tekintse meg a következő dokumentumok hello:
* [Diagnosztikai adatok tárolása és megtekintése az Azure Storage-ban](cloud-services-dotnet-diagnostics-storage.md)
* [IIS-napló leállítja a felhőszolgáltatást írt](https://blogs.msdn.microsoft.com/cie/2013/12/21/iis-logs-stops-writing-in-cloud-service/)

## <a name="what-is-hello-purpose-of-hello-windows-azure-tools-encryption-certificate-for-extensions"></a>Mi az a "Windows Azure eszközök titkosítási a kiegészítő" hello hello célját?
Ezek a tanúsítványok automatikusan jönnek létre, amikor kiterjesztés kerül toohello felhőalapú szolgáltatás. Leggyakrabban hello ÜVEGVATTA bővítmény vagy hello RDP-bővítményt, de annak oka az lehet, például hello kártevőirtó vagy naplógyűjtő bővítmény. Ezek a tanúsítványok csak használt titkosítására és visszafejtésére hello kiterjesztés hello titkos konfigurációját. hello lejárati dátum soha nem ellenőrzi, tehát nem számít, ha hello tanúsítvány lejárt. 

Ezek a tanúsítványok figyelmen kívül hagyhatja. Ha azt szeretné, hello tanúsítványok karbantartása, próbálja meg őket az összes törlése. Azure kivételhibát hiba toodelete jelszómódosítás egy tanúsítványt, amely használatban van.

## <a name="how-can-i-generate-a-certificate-signing-request-csr-without-rdp-ing-in-toohello-instance"></a>Hogyan hozhat létre toohello példányban egy tanúsítvány-aláírási kérelem (CSR) "RDP-ing" nélkül?
Tekintse meg a következő útmutató hello:

>[Megszerezni a tanúsítványt a Windows Azure-webhelyek (WAWS)](https://azure.microsoft.com/blog/obtaining-a-certificate-for-use-with-windows-azure-web-sites-waws/)

Vegye figyelembe, hogy a CSR-e egy szövegfájlt. Nincs hello gépből létrehozott toobe amelyeken hello tanúsítvány végső soron kell használni. Bár ez a dokumentum írása egy App Service, a hello CSR létrehozása általános, és a Felhőszolgáltatások is vonatkozik.
