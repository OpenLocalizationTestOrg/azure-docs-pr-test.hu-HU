---
title: "Munka a meglévő helyszíni proxykiszolgálókra és az Azure AD |} Microsoft Docs"
description: "Bemutatja, hogyan adhat az meglévő helyszíni proxy kiszolgálóhoz."
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
ms.date: 08/04/2017
ms.author: kgremban
ms.openlocfilehash: bdca442755507c4ffe8d43692c5b7f2aa3a746f3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="work-with-existing-on-premises-proxy-servers"></a>A meglévő helyszíni proxykiszolgálókkal működik

Ez a cikk ismerteti, hogyan konfigurálja az Azure Active Directory (Azure AD) alkalmazásproxy összekötőket az kimenőproxy-kiszolgálóhoz. Az ügyfelek számára hálózati környezetekben, ahol a meglévő proxyk szolgál.

Először fő központi telepítési forgatókönyvek alapján:
* Konfigurálja az összekötőket a helyszíni kimenő proxyk kihagyásához.
* Konfigurálja az összekötőket egy kimenő proxy használatát az Azure AD-alkalmazásproxy eléréséhez.

Összekötők működésével kapcsolatos további információkért lásd: [megértéséhez Azure AD-alkalmazásproxy összekötők](application-proxy-understand-connectors.md).

## <a name="configure-the-outbound-proxy"></a>A kimenő proxy konfigurálása

Ha egy kimenő proxy a környezetben, fiók használatával a megfelelő engedélyekkel a kimenő proxy konfigurálása. A telepítő a telepítés tesz a felhasználó környezetében fut, mert a konfiguráció ellenőrizheti a Microsoft Edge vagy egy másik böngészőben.

A Proxybeállítások konfigurálása a Microsoft Edge:

1. Ugrás **beállítások** > **nézet speciális beállítások** > **nyissa meg a proxybeállítások** > **manuálisProxytelepítése**.
2. Állítsa be **proxykiszolgálót használni** való **a**, jelölje be a **nem használni a proxykiszolgálót a helyi (intranetes) címek** jelölőnégyzetet, majd módosítsa a címet és portot a helyi megfelelően proxykiszolgáló.
3. Töltse ki a szükséges proxybeállításokat.

   ![A proxykiszolgáló beállításai párbeszédpanel](./media/application-proxy-working-with-proxy-servers/proxy-bypass-local-addresses.png)

## <a name="bypass-outbound-proxies"></a>Kimenő proxyk figyelmen kívül hagyása

Összekötők rendelkezik az operációs rendszer alapösszetevőt kimenő kérelmeket. Ezeket az összetevőket automatikusan megpróbálja megkeresni a proxykiszolgálót a hálózaton. Webes Proxy automatikus felderítését a lekérdezés (WPA), akkor használja, ha engedélyezve van a környezetben.

Az operációs rendszer összetevőit megpróbálja megkeresni a proxykiszolgáló wpad.domainsuffix a DNS-címkeresést elvégzésével. Ha így megoldódik, a DNS-ben, HTTP-kérelem majd történik wpad.dat IP-címét. A kérelem válik a proxy konfigurációs parancsprogramja a környezetben. Az összekötő jelöljön ki egy kimenő proxy használja ezt a parancsfájlt. Azonban összekötő forgalom előfordulhat, hogy még nem halad át, a proxy szükség további konfigurációs beállítások miatt.

Beállíthatja, hogy az összekötő a helyszíni proxy győződjön meg arról, hogy használ-e közvetlen kapcsolat az Azure-szolgáltatásokhoz való kihagyásához. Ezt a módszert javasoljuk (Ha a hálózati házirend lehetővé teszi, hogy az), mert az azt jelenti, hogy rendelkezik-e egy kisebb konfigurációs fenntartásához.

Tiltsa le az összekötő kimenő proxy használatát, a C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config fájl szerkesztésével, és adja hozzá a *System.NET névtérbeli* látható a fenti szakaszban:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.net>
    <defaultProxy enabled="false"></defaultProxy>
  </system.net>
  <runtime>
    <gcServer enabled="true"/>
  </runtime>
  <appSettings>
    <add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```
Győződjön meg arról, hogy az összekötő frissítési szolgáltatást is megkerüli a proxy, egy hasonló módosítást a ApplicationProxyConnectorUpdaterService.exe.config fájl helye: C:\Program Files\Microsoft AAD App Proxy Connector Frissítőjének.

Ügyeljen arra, hogy készítsen másolatot az eredeti fájlok abban az esetben meg kell visszaállítania az alapértelmezett .config kiterjesztésű fájlokat.

## <a name="use-the-outbound-proxy-server"></a>A kimenő proxykiszolgáló használata

Egyes környezetekben megkövetelése minden kimenő forgalom haladhat végig az kimenő proxy, kivételhiba jelentkezése nélkül. Ennek eredményeképpen a proxy megkerülése lehetőség nem érhető el.

Nyissa meg a kimenő proxy connector forgalmat is konfigurálhatja, az alábbi ábrán látható módon:

 ![Egy kimenő proxyn keresztül történő Ugrás az Azure AD-alkalmazásproxy-összekötő forgalom konfigurálása](./media/application-proxy-working-with-proxy-servers/configure-proxy-settings.png)

Miatt, amelyek csak a kimenő forgalom, nincs szükség a tűzfalon keresztüli bejövő hozzáférés konfigurálásához.

### <a name="step-1-configure-the-connector-and-related-services-to-go-through-the-outbound-proxy"></a>1. lépés: Az összekötő és a kapcsolódó szolgáltatások haladhat végig a kimenő proxy konfigurálása

Korábban vonatkozik, ha WPAD engedélyezve a környezetben, és megfelelően konfigurálva, az összekötő automatikusan a kimenő proxykiszolgáló felderítése, és megkísérli használni azt. Azonban explicit módon konfigurálhatja az összekötő egy kimenő proxy végrehajtania.

Ehhez az szükséges, módosítsa a C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config fájlt, és adja hozzá a *System.NET névtérbeli* látható a fenti szakaszban. Változás *proxyserver:8080* a helyi proxykiszolgáló nevét vagy IP-cím és a portot, amelyet figyeli az megfelelően.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.net>  
    <defaultProxy>   
      <proxy proxyaddress="http://proxyserver:8080" bypassonlocal="True" usesystemdefault="True"/>   
    </defaultProxy>  
  </system.net>
  <runtime>
    <gcServer enabled="true"/>
  </runtime>
  <appSettings>
    <add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```

Ezután konfigurálja a Connector frissítési szolgáltatást által hasonló módosítást végez a fájl helye: C:\Program Files\Microsoft AAD App Proxy Connector Updater\ApplicationProxyConnectorUpdaterService.exe.config proxy használatára.

### <a name="step-2-configure-the-proxy-to-allow-traffic-from-the-connector-and-related-services-to-flow-through"></a>2. lépés: Az összekötő és a kapcsolódó szolgáltatások keresztül érkező forgalmat engedélyezi a proxy konfigurálása

Négy szempontot kell figyelembe venni a kimenő proxynál:
* Proxy kimenő szabályok
* A proxy hitelesítése
* Proxy portok
* SSL-ellenőrzést

#### <a name="proxy-outbound-rules"></a>Proxy kimenő szabályok
Az összekötő szolgáltatás hozzáférés a következő végpontok hozzáférés engedélyezése:

* *. msappproxy.net
* *. servicebus.windows.net

A kezdeti regisztráció a következő végpontok hozzáférés engedélyezése:

* login.windows.net
* login.microsoftonline.com

Ha nem engedélyezi a csatlakozást a teljes tartománynév alapján és IP-címtartományok Ehelyett meg kell adni, használja ezeket a beállításokat:

* Az összes cél összekötő kimenő hozzáférés engedélyezése.
* Az összekötő kimenő hozzáférést nyújtson [Azure datacenter IP-címtartományok](https://www.microsoft.com/en-gb/download/details.aspx?id=41653). Használatával a Azure datacenter IP-címtartománylista ellenőrző, hogy hetente kell frissíteni. Győződjön meg arról, hogy a hozzáférési szabályok megfelelően vannak-e frissíti a folyamat bevezetni kell.

#### <a name="proxy-authentication"></a>A proxy hitelesítése

A proxy hitelesítése jelenleg nem támogatott. Az aktuális javasoljuk, hogy az összekötő a Internet célhelyre névtelen hozzáférés engedélyezése.

#### <a name="proxy-ports"></a>Proxy portok

Az összekötő lehetővé teszi a kimenő SSL-alapú kapcsolatok KAPCSOLATI módszer használatával. Ez a módszer lényegében állít be egy alagúton, a kimenő proxyn keresztül. Engedélyezi a 443-as és a 80-as portra tunneling a proxykiszolgáló konfigurálása.

>[!NOTE]
>A Service Bus a HTTPS-KAPCSOLATON keresztül futtatása 443-as portot használja. Azonban az alapértelmezés szerint a Service Bus kísérletet tett a közvetlen TCP-kapcsolatok, és csak akkor, ha közvetlen kapcsolatot sikertelen visszaáll HTTPS.

Győződjön meg arról, hogy a Service Bus-forgalom is zajlik a kimenő proxykiszolgálón keresztül, győződjön meg arról, hogy az összekötő nem közvetlenül csatlakozni az Azure-szolgáltatások 9350 9352 és 5671 portok.

#### <a name="ssl-inspection"></a>SSL-ellenőrzést
Ne használjon SSL-ellenőrzést az összekötő-forgalom esetén, mert az összekötő forgalom problémákat okoz.

## <a name="troubleshoot-connector-proxy-problems-and-service-connectivity-issues"></a>Összekötő proxy problémák és a szolgáltatás kapcsolódási problémák elhárítása
Most már megtekintheti az összes forgalom a proxyn keresztül történő továbbítására. Ha problémába ütközik, a következő hibaelhárítási információk segítségül szolgálhat.

A legjobb azonosítása és összekötő csatlakozási problémák módja lehet az összekötő szolgáltatás hálózati rögzítőeszközt végrehajtani az összekötő-szolgáltatás indításakor. Ez nagyobb terhet jelenthetnek, gyors tippek az rögzítésével és hálózati nyomkövetés szűréshez tehát vizsgáljuk meg.

A felügyeleti eszköz az Ön által választott használható. Ez a cikk alkalmazásában Microsoft Network Monitor 3.4 használtuk. Is [töltse le a Microsoft](https://www.microsoft.com/download/details.aspx?id=4865).

A példák és szűrőket, amelyek a következő szakaszokban használjuk csak az adott hálózati figyelő, de bármely eszköz alkalmazhatja ezeket az alapelveket.

### <a name="take-a-capture-by-using-network-monitor"></a>Tegye meg a rögzítés a Hálózatfigyelővel

A Rögzítés indítása:

1. Nyissa meg a hálózati figyelő, és kattintson a **új rögzítése**.
2. Kattintson a **Start** gombra.

   ![Hálózati figyelő ablak](./media/application-proxy-working-with-proxy-servers/network-capture.png)

Miután elkészült egy rögzítési, kattintson a **leállítása** gombra kell végződnie.

### <a name="take-a-capture-of-connector-traffic"></a>Az összekötő forgalom rögzítés igénybe

A kezdeti hibaelhárítási, hajtsa végre az alábbi lépéseket:

1. A "Services.msc" parancsot állítsa le az Azure AD alkalmazásproxy-összekötő szolgáltatást.
2. Indítsa el a hálózati rögzítési.
3. Indítsa el az Azure AD alkalmazásproxy-összekötő szolgáltatást.
4. Állítsa le a hálózati rögzítést.

   ![A "Services.msc" parancsot az Azure AD alkalmazásproxy-összekötő szolgáltatás](./media/application-proxy-working-with-proxy-servers/services-local.png)

### <a name="look-at-the-requests-from-the-connector-to-the-proxy-server"></a>Tekintse meg a kérelmeket az összekötőről érkező a proxykiszolgálóhoz

Most, hogy van hálózati rögzítőeszközt, készen áll az szűrése. A kulcsot, a nyomkövetés megnézi a rögzítés szűrése van ismertetése.

Egy szűrő értéke (ahol a 8080-as azt a szolgáltatás proxyport):

**(http-alapú. - Vagy HTTP-kérelem. Válasz) és tcp.port==8080**

Ha a szűrőt a adja meg a **szűrő megjelenítése** ablakot, és válassza ki **alkalmaz**, a rögzített forgalom megadott szűrő alapján szűri.

Az előző szűrő csak a HTTP-kérések és válaszok jeleníti meg a proxykiszolgáló port és a. Az összekötő indítás, ha az összekötő egy proxykiszolgáló használatára van konfigurálva a szűrő megjelenítése volna például ehhez hasonló:

 ![Szűrt HTTP-kérések és válaszok példa listája](./media/application-proxy-working-with-proxy-servers/http-requests.png)

Mostantól kifejezetten keres a CONNECT-kérelmeket, amelyek megjelenítik a proxy-kiszolgálóval folytatott kommunikációhoz. Sikeres, akkor egy HTTP-OK (200-as) választ kapott.

Ha megjelenik a többi válaszkódnál 407 vagy 502-es, például a proxy hitelesítést igénylő vagy nem engedélyezi a forgalom valamilyen más okból. Ezen a ponton bevonása, akik a proxy server támogatási csoportjához.

### <a name="identify-failed-tcp-connection-attempts"></a>Sikertelen TCP-kapcsolati kísérletek azonosítása

A többi gyakori forgatókönyv, előfordulhat, hogy érdeklődik akkor, ha az összekötő közvetlenül csatlakozni próbál, de ez nem működik.

Egy másik hálózati figyelő szűrő, amellyel könnyedén azonosíthatja a probléma a következő:

**tulajdonság. TCPSynRetransmit**

SZIN csomagot küldött egy TCP-kapcsolatot létesíteni az első csomag. Ez a csomag a válasz nem ad vissza, ha a szinkronizálás a mi reattempted van. Az előző szűrő segítségével bármely újraküldött SYNs láthatja. Ezután ellenőrizheti, hogy ezek SYNs megfelelnek-e a connector kapcsolatos forgalommal.

A következő példa bemutatja a Service Bus-portra 9352 sikertelen kapcsolódási kísérlet:

 ![Példa egy válasz sikertelen a rendszer](./media/application-proxy-working-with-proxy-servers/failed-connection-attempt.png)

Ha például az előző választ, az összekötő megpróbál közvetlenül az Azure Service Bus szolgáltatással való kommunikációra. Ha várhatóan az Azure-szolgáltatásokhoz való közvetlen kapcsolat az összekötőt, ezt a választ, hogy rendelkezik-e a hálózati vagy probléma egyértelmű jelzése.

>[!NOTE]
>A proxykiszolgáló használatára van konfigurálva, a válasz jelentheti, hogy a Service Bus kísérel meg a közvetlen TCP-kapcsolat HTTPS-KAPCSOLATON keresztüli kapcsolatot próbált átváltás előtt.
>

A hálózati nyomkövetés elemzésének nincs mindenki számára. De mi a helyzet a hálózattal kapcsolatos gyors adatok hasznos eszköze lehet.

Ha továbbra is az összekötő csatlakozási problémák kihívást jelent, hozzon létre a jegyet a támogatási csapat. A csapatunk segítségére a további hibaelhárításhoz.

Az alkalmazásproxy-összekötő hibák feloldására vonatkozó információkért lásd: [alkalmazásproxy hibaelhárítása](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-troubleshoot).

## <a name="next-steps"></a>Következő lépések

[Az Azure AD-alkalmazásproxy összekötők ismertetése](application-proxy-understand-connectors.md)<br>
[Csendes telepítése az Azure AD alkalmazásproxy-összekötő](active-directory-application-proxy-silent-installation.md)
