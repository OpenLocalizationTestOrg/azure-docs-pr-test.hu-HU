---
title: "a meglévő aaaWork helyi proxykiszolgálót és az Azure AD |} Microsoft Docs"
description: "Ismerteti, hogyan toowork a meglévő helyszíni proxykiszolgálókat."
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
ms.openlocfilehash: 7f8cec4f676f99bead5211bcbcf23056bd7f211f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="work-with-existing-on-premises-proxy-servers"></a>A meglévő helyszíni proxykiszolgálókkal működik

Ez a cikk azt ismerteti, hogyan tooconfigure Azure Active Directory (Azure AD) alkalmazásproxy összekötők toowork kimenőproxy-kiszolgálókkal. Az ügyfelek számára hálózati környezetekben, ahol a meglévő proxyk szolgál.

Először fő központi telepítési forgatókönyvek alapján:
* Összekötők toobypass konfigurálása a helyszíni kimenő proxyk.
* Egy kimenő proxy tooaccess Azure AD alkalmazásproxy összekötők toouse konfigurálása.

Összekötők működésével kapcsolatos további információkért lásd: [megértéséhez Azure AD-alkalmazásproxy összekötők](application-proxy-understand-connectors.md).

## <a name="configure-hello-outbound-proxy"></a>Hello kimenő proxy konfigurálása

Ha egy kimenő proxy a környezetben, a megfelelő engedélyek tooconfigure hello kimenő proxy olyan fiókot használjon. Hello telepítő hello hello tesz hello telepítés a felhasználó környezetében fut, mert a Microsoft Edge vagy egy másik böngészőben ellenőrizheti hello beállításait.

tooconfigure hello proxybeállításai Microsoft Edge:

1. Nyissa meg túl**beállítások** > **speciális beállítások megjelenítése** > **nyissa meg a proxybeállítások** > **manuális Proxy telepítése** .
2. Állítsa be **proxykiszolgálót használni** túl**a**, jelölje be hello **ne használjon hello proxykiszolgáló helyi (intranetes) címek** jelölőnégyzetet, majd a módosítás hello cím és port tooreflect a helyi proxykiszolgálóra.
3. Töltse ki hello szükséges proxybeállításokat.

   ![A proxykiszolgáló beállításai párbeszédpanel](./media/application-proxy-working-with-proxy-servers/proxy-bypass-local-addresses.png)

## <a name="bypass-outbound-proxies"></a>Kimenő proxyk figyelmen kívül hagyása

Összekötők rendelkezik az operációs rendszer alapösszetevőt kimenő kérelmeket. Ezeket az összetevőket automatikusan megpróbál toolocate hello hálózat proxykiszolgálót. Webes Proxy automatikus felderítését a lekérdezés (WPA), ha engedélyezve van a hello környezet használata.

hello operációs rendszer összetevőit toolocate proxykiszolgáló wpad.domainsuffix a DNS-címkeresést elvégzésével történt kísérlet. Ha így megoldódik, a DNS-ben, egy HTTP majd kérelem toohello IP-címe wpad.dat. A kérelem lesz hello proxy konfigurációs parancsprogramja a környezetben. hello összekötő a parancsfájl tooselect egy kimenő proxykiszolgálót használ. Azonban összekötő forgalom előfordulhat, hogy még nem halad át, hello proxy szükség további konfigurációs beállítások miatt.

Konfigurálhatja a helyszíni proxy tooensure használt közvetlen kapcsolat toohello Azure hello összekötő toobypass szolgáltatások. Ezt a módszert javasoljuk (Ha a hálózati házirend lehetővé teszi, hogy az), mert az azt jelenti, hogy rendelkezik-e egy kisebb konfigurációs toomaintain.

toodisable kimenőproxy használati hello összekötő hello C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config fájl szerkesztésével, és adja hozzá a hello *System.NET névtérbeli* látható a fenti szakasz :

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
tooensure, hogy hello összekötő frissítési szolgáltatást is megkerüli hello proxy, hogy hasonló módosítása toohello ApplicationProxyConnectorUpdaterService.exe.config fájl helye: C:\Program Files\Microsoft AAD App Proxy Connector Frissítőjének.

Hello eredeti fájlok másolatait meg arról, hogy toomake lennie, ha toorevert toohello alapértelmezett .config kiterjesztésű fájlokban van szüksége.

## <a name="use-hello-outbound-proxy-server"></a>Hello kimenő proxykiszolgáló használata

Egyes környezetekben megkövetelése minden kimenő forgalom toogo keresztül kimenő, kivételhiba jelentkezése nélkül. Ennek eredményeképpen hello proxy megkerülése lehetőség nem érhető el.

Hello összekötő forgalom toogo hello kimenő proxyn keresztül is konfigurálhatja, ahogy az ábra a következő hello:

 ![Konfigurálása az összekötő forgalom toogo keresztül egy kimenő proxy tooAzure AD alkalmazásproxy](./media/application-proxy-working-with-proxy-servers/configure-proxy-settings.png)

Ennek eredményeképpen, hogy csak a kimenő forgalom, nincs nincs szükség tooconfigure hozzáférés a tűzfalon keresztüli bejövő.

### <a name="step-1-configure-hello-connector-and-related-services-toogo-through-hello-outbound-proxy"></a>1. lépés: Hello-összekötő konfigurálása, és a kapcsolódó szolgáltatások toogo hello kimenő proxyn keresztül

Vonatkozik korábban, ha a WPAD hello környezetben engedélyezve van, és megfelelő konfigurálása, mint a hello connector automatikusan észleli az hello kimenőproxy kiszolgáló, és megpróbál toouse azt. Azonban explicit módon konfigurálhatja hello összekötő toogo egy kimenő proxyn keresztül.

toodo tehát hello C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config fájl szerkesztésével, és adja hozzá a hello *System.NET névtérbeli* látható a fenti szakaszban. Változás *proxyserver:8080* a helyi proxykiszolgáló nevét vagy IP-cím és hello porton figyeli a tooreflect.

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

Ezután konfigurálja a hello összekötő Frissítőjének toouse hello szolgáltatásproxy azáltal, hogy a hasonló módosítása toohello fájl helye: C:\Program Files\Microsoft AAD App Proxy Connector Updater\ApplicationProxyConnectorUpdaterService.exe.config.

### <a name="step-2-configure-hello-proxy-tooallow-traffic-from-hello-connector-and-related-services-tooflow-through"></a>2. lépés: Hello proxy tooallow forgalom hello összekötő és a kapcsolódó szolgáltatások tooflow keresztül konfigurálása

Jelenleg hello kimenőproxy négy szempontok tooconsider:
* Proxy kimenő szabályok
* A proxy hitelesítése
* Proxy portok
* SSL-ellenőrzést

#### <a name="proxy-outbound-rules"></a>Proxy kimenő szabályok
A következő hozzáférést összekötő-szolgáltatás végpontjainak hozzáférés toohello engedélyezése:

* *. msappproxy.net
* *. servicebus.windows.net

A kezdeti regisztráció a következő végpontok hozzáférés toohello engedélyezése:

* login.windows.net
* login.microsoftonline.com

Ha nem engedélyezi a kapcsolatot a teljes tartománynév alapján, és kell toospecify IP-címtartományok Ehelyett használja az alábbi beállításokat:

* Hello összekötő kimenő hozzá lehessen férni tooall célhelyre.
* Hozzáférést hello összekötő kimenő túl[Azure datacenter IP-címtartományok](https://www.microsoft.com/en-gb/download/details.aspx?id=41653). hello challenge hello Azure datacenter IP-címtartománylista használatával, hogy hetente kell frissíteni. Egy folyamatot, hogy a hozzáférési szabályok megfelelően vannak-e frissíti hely tooensure tooput van szüksége.

#### <a name="proxy-authentication"></a>A proxy hitelesítése

A proxy hitelesítése jelenleg nem támogatott. Az aktuális ajánlott megoldás tooallow hello összekötő névtelen hozzáférés toohello Internet célok.

#### <a name="proxy-ports"></a>Proxy portok

hello összekötő lehetővé teszi a kimenő SSL-alapú kapcsolatok hello KAPCSOLATI módszer használatával. Ez a módszer lényegében állít be egy alagúton hello kimenő proxyn keresztül. Hello proxy server tooallow tooports 443-as és a 80-as bújtatás konfigurálása.

>[!NOTE]
>A Service Bus a HTTPS-KAPCSOLATON keresztül futtatása 443-as portot használja. Azonban az alapértelmezés szerint a Service Bus kísérletet tett a közvetlen TCP-kapcsolatok, és visszaáll tooHTTPS csak akkor, ha közvetlen kapcsolatot sikertelen.

Service Bus forgalom is zajlik hello kimenő proxykiszolgálón keresztül hello, győződjön meg arról, hogy hello összekötő közvetlenül nem lehet kapcsolódni a toohello tooensure Azure szolgáltatások 9350 9352 és 5671 portok.

#### <a name="ssl-inspection"></a>SSL-ellenőrzést
Ne használjon SSL-ellenőrzést hello összekötő forgalmat, mert hello összekötő forgalom problémákat okoz.

## <a name="troubleshoot-connector-proxy-problems-and-service-connectivity-issues"></a>Összekötő proxy problémák és a szolgáltatás kapcsolódási problémák elhárítása
Most már megtekintheti az összes forgalom hello proxyn keresztül történő továbbítására. Ha problémába ütközik, a következő hibaelhárítási információk hello segítségül szolgálhat.

legjobb módja tooidentify hello és összekötő kapcsolatok problémák tootake hello összekötő szolgáltatás indítása közben hello összekötő szolgáltatás egy hálózati adatváltozások rögzítése. Ez nagyobb terhet jelenthetnek, gyors tippek az rögzítésével és hálózati nyomkövetés szűréshez tehát vizsgáljuk meg.

A kiválasztott eszköz figyelési hello is használhatja. Ez a cikk hello célokra Microsoft Network Monitor 3.4 használtuk. Is [töltse le a Microsoft](https://www.microsoft.com/download/details.aspx?id=4865).

hello példák és szűrőket, amelyek a következő részekben hello használjuk adott tooNetwork figyelő, de hello alapelvek lehet alkalmazott tooany elemző eszközt.

### <a name="take-a-capture-by-using-network-monitor"></a>Tegye meg a rögzítés a Hálózatfigyelővel

a rögzítési toostart:

1. Nyissa meg a hálózati figyelő, és kattintson a **új rögzítése**.
2. Kattintson a hello **Start** gombra.

   ![Hálózati figyelő ablak](./media/application-proxy-working-with-proxy-servers/network-capture.png)

A rögzítési befejezése után kattintson a hello **leállítása** gomb tooend azt.

### <a name="take-a-capture-of-connector-traffic"></a>Az összekötő forgalom rögzítés igénybe

A kezdeti hibaelhárítási, hajtsa végre a lépéseket követve hello:

1. A "Services.msc" parancsot hello Azure AD alkalmazásproxy-összekötő szolgáltatás leállítása.
2. Indítsa el a hello hálózati rögzítési.
3. Hello Azure AD alkalmazásproxy-összekötő szolgáltatás elindítása.
4. Állítsa le a hello hálózati rögzítést.

   ![A "Services.msc" parancsot az Azure AD alkalmazásproxy-összekötő szolgáltatás](./media/application-proxy-working-with-proxy-servers/services-local.png)

### <a name="look-at-hello-requests-from-hello-connector-toohello-proxy-server"></a>Nézze meg hello kérelmek hello összekötő toohello proxykiszolgáló

Most, hogy van hálózati rögzítőeszközt, most készen áll a toofilter azt. hello kulcs toolooking: hello nyomkövetési van megértése, hogyan toofilter hello rögzítése.

Egy szűrő értéke (ahol a 8080-as azt hello szolgáltatás proxyport):

**(http-alapú. - Vagy HTTP-kérelem. Válasz) és tcp.port==8080**

Ha ez a szűrő ír hello **szűrő megjelenítése** ablakot, és válassza ki **alkalmaz**, rögzített hello forgalom hello szűrő alapján szűri.

hello előző szűrő látható az ebben az esetben hello HTTP-kérések és válaszok hello proxy portja és a. Hello összekötő esetén konfigurált toouse proxykiszolgálón connector indítási, az alábbihoz hasonló hello szűrő volna megjelenítése:

 ![Szűrt HTTP-kérések és válaszok példa listája](./media/application-proxy-working-with-proxy-servers/http-requests.png)

Mostantól kifejezetten keresett hello csatlakozás igényel, amely megjelenítése hello proxy-kiszolgálóval folytatott kommunikációhoz. Sikeres, akkor egy HTTP-OK (200-as) választ kapott.

Ha megjelenik a többi válaszkódnál, mint a 407 vagy 502-es, hello proxy hitelesítést igénylő vagy nem így bármilyen más okból hello forgalmat. Ezen a ponton bevonása, akik a proxy server támogatási csoportjához.

### <a name="identify-failed-tcp-connection-attempts"></a>Sikertelen TCP-kapcsolati kísérletek azonosítása

hello egyéb gyakori forgatókönyv, előfordulhat, hogy érdeklődik esetén hello összekötő próbál tooconnect közvetlenül, de ez nem működik.

Van egy másik hálózati figyelő szűrő, amely segít tooeasily, ez a probléma meghatározásához:

**tulajdonság. TCPSynRetransmit**

A szinkronizálás a mi csomag érték hello első csomag tooestablish TCP-kapcsolatot. Ha a csomag nem ad vissza a választ, hello SZIN reattempted van. Szűrő toosee megelőző hello bármely újraküldött SYNs használható. Ezután ellenőrizheti, hogy ezek SYNs megfelelnek-e tooany összekötő kapcsolatos forgalom.

hello alábbi példa bemutatja a sikertelen kapcsolódás tooService Bus port 9352:

 ![Példa egy válasz sikertelen a rendszer](./media/application-proxy-working-with-proxy-servers/failed-connection-attempt.png)

Ha például a válasz megelőző hello, hello összekötő megpróbál közvetlenül az Azure Service Bus szolgáltatás hello toocommunicate. Ha a várt hello összekötő toomake közvetlen kapcsolatokon keresztül toohello Azure szolgáltatások, ezt a választ az, hogy rendelkezik-e a hálózati vagy probléma egyértelmű jelzése.

>[!NOTE]
>Ha konfigurált toouse proxykiszolgáló, ez a válasz jelentheti, hogy a Service Bus kísérel meg előtt váltás tooattempting a kapcsolat HTTPS-KAPCSOLATON keresztül a közvetlen TCP-kapcsolat.
>

A hálózati nyomkövetés elemzésének nincs mindenki számára. Azonban a legértékesebb eszköz tooget gyors információ a hálózati jelenít meg.

Ha Ön most folytatja, az összekötő csatlakozási problémák toostruggle, hozzon létre egy jegyet a támogatási csapat. hello csapatunk segítségére a további hibaelhárításhoz.

Az alkalmazásproxy-összekötő hibák feloldására vonatkozó információkért lásd: [alkalmazásproxy hibaelhárítása](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-troubleshoot).

## <a name="next-steps"></a>Következő lépések

[Az Azure AD-alkalmazásproxy összekötők ismertetése](application-proxy-understand-connectors.md)<br>
[Hogyan a toosilently hello Azure AD alkalmazásproxy-összekötő telepítése](active-directory-application-proxy-silent-installation.md)
