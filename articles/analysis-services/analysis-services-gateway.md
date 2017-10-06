---
title: "aaaOn helyszíni adatátjáró |} Microsoft Docs"
description: "Egy helyszíni átjárót szükség, ha az Analysis Services-kiszolgálóhoz, az Azure-ban tooon helyi adatforrások fog csatlakozni."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: cd596155-b608-4a34-935e-e45c95d884a9
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/21/2017
ms.author: owend
ms.openlocfilehash: fc7b9c69e6f81b41deb7a5d6d963225593845d84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-tooon-premises-data-sources-with-azure-on-premises-data-gateway"></a>Helyszíni Azure Data Gateway tooon helyi adatforrások összekapcsolása
hello helyszíni adatátjáró hídként, a helyszíni adatforrások és a hello felhőben Azure Analysis Services-kiszolgálók közötti biztonságos adatátvitel biztosító működik. Továbbá tooworking hello a több Azure Analysis Services-kiszolgáló ugyanabban a régióban, hello hello átjáró legújabb verziója is működik az Azure Logic Apps, a Power bi-ban, a kiemelt alkalmazások és a Microsoft Flow. A hello több szolgáltatáshoz is társíthat ugyanabban a régióban, ha csak egyetlen átjáró. 

 Az Azure Analysis Services egy átjáró erőforrás hello szükséges ugyanabban a régióban. Például ha Azure Analysis Services-kiszolgálók hello USA keleti régiója 2 régióban, kell egy átjáró-erőforráshoz régióban hello USA keleti régiója 2. USA keleti régiója 2 több kiszolgáló használható hello ugyanahhoz az átjáróhoz.

Első hello átjáró hello telepítése először az egy négyrészes folyamat:

- **Töltse le és futtassa a telepítőt** – Ez a lépés telepíti egy átjáró szolgáltatás a szervezet egyik számítógépén.

- **Regisztrálnia kell az átjárót** – ebben a lépésben adjon nevet és helyreállítási az átjáró kulcsát, és válasszon ki egy régiót, az átjáró regisztrálása hello átjáró Felhőszolgáltatáshoz.

- **Hozzon létre egy átjáró erőforrást az Azure-ban** -ebben a lépésben az Azure-előfizetése létrehozhat egy átjáró-erőforráshoz.

- **Csatlakozás a kiszolgálók tooyour átjáró erőforrás** -előfizetése van egy átjáró-erőforráshoz, amennyiben a kiszolgálók tooit kapcsolódás megkezdheti.

Ha már van egy átjáró-erőforráshoz az előfizetéséhez beállított, több kiszolgáló, és egyéb szolgáltatások tooit is elérheti. Csak egy másik átjáró tooinstall kell, és további átjáró erőforrások létrehozása, ha más szolgáltatások vagy kiszolgálók egy másik régióban.

azonnal, indítható tooget lásd [telepítése és konfigurálása a helyszíni adatátjáró](analysis-services-gateway-install.md).

## <a name="how-it-works"></a>Annak működéséről
a Windows szolgáltatásként fut egy számítógépre a szervezet telepít hello átjáró **helyszíni adatátjáró**. A helyi szolgáltatás hello átjáró Felhőszolgáltatáshoz Azure Service Buson keresztül van regisztrálva. Ezután hozzon létre egy átjáró-erőforráshoz átjáró Felhőszolgáltatáshoz Azure-előfizetése. Az Azure Analysis Services-kiszolgálók esetén, majd csatlakoztatott tooyour átjáró-erőforráshoz. Ha a kiszolgáló kell tooconnect tooyour modell a helyszíni adatforrások lekérdezések vagy feldolgozásra, a lekérdezés- és folyamata traverses hello átjáró erőforrások Azure Service Bus hello helyi helyszíni átjáró szolgáltatás, és az adatforrások. 

![Működés](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Lekérdezések és adatfolyam:

1. A lekérdezés hello felhőszolgáltatás hello titkosított hitelesítő adatokkal a helyszíni adatforrás hello hozta létre. Majd továbbított hello átjáró tooprocess tooa várakozási sorban.
2. hello átjáró felhőszolgáltatáshoz hello lekérdezés elemzi, és leküldéses értesítések hello kérelem toohello [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).
3. hello helyszíni adatátjáró hello Azure Service Bus a függőben lévő kérelmek kérdezi le.
4. hello átjáró hello lekérdezés lekérdezi, visszafejti hello hitelesítő adatokat, és toohello adatforrásokhoz csatlakoznak az ezeket a hitelesítő adatokat.
5. hello átjáró küldi hello lekérdezési toohello adatforrás végrehajtásra.
6. hello eredmények küldött hello adatforrások, hátsó toohello átjáró, és hello felhőalapú szolgáltatás, és a kiszolgáló.

## <a name="windows-service-account"></a>Windows-szolgáltatás fiókja
hello helyszíni az adatátjáró konfigurált toouse *NT SERVICE\PBIEgwService* a hello Windows szolgáltatás bejelentkezési hitelesítő adatait. Alapértelmezés szerint rendelkezik hello jobb szélén bejelentkezés szolgáltatásként; hello gép, amely telepíti az hello átjáró hello környezetében. Ezeket a hitelesítő adatokat nem hello, azonos használt fiók tooconnect tooon helyi adatforrások vagy az Azure-fiókjával.  

Ha problémák miatt a proxykiszolgáló tooauthentication, érdemes lehet toochange hello Windows szolgáltatás tooa tartományfelhasználói fiókot, vagy felügyelt szolgáltatásfiók.

## <a name="ports"></a>Portok
hello átjáró létrehoz egy kimenő kapcsolat tooAzure Service Bus. A kimenő portokon kommunikál: TCP 443-as (alapértelmezett), 5671, 5672, 9350 – 9354-es.  hello átjáró nincs szükség a bejövő portra.

Javasoljuk, hogy engedélyezett hello IP-címet az adatterületen, a tűzfalon. Letöltheti a hello [Microsoft Azure Datacenter IP-lista](https://www.microsoft.com/download/details.aspx?id=41653). A lista a heti frissül.

> [!NOTE]
> hello IP-címek szerepel-e hello Azure Datacenter IP CIDR-formátumban vannak. Például: 10.0.0.0/24 nem jelenti azt 10.0.0.0 10.0.0.24 keresztül. További tudnivalók hello [CIDR-jelöléssel](http://whatismyipaddress.com/cidr).
>
>

Az alábbiakban hello hello átjáró által használt hello teljesen minősített tartománynév.

| Tartománynevek | Kimenő portok | Leírás |
| --- | --- | --- |
| *. powerbi.com webhelyre |80 |HTTP toodownload hello telepítő használt. |
| *. powerbi.com webhelyre |443 |HTTPS |
| *. analysis.windows.net |443 |HTTPS |
| *. login.windows.net |443 |HTTPS |
| *. servicebus.windows.net |5671-5672 |Speciális üzenetsor-kezelési protokoll (AMQP) |
| *. servicebus.windows.net |443, 9350-9354 |A Service Bus Relay (a 443-as kér a hozzáférés-vezérlés jogkivonat beszerzése) TCP-n keresztül figyelői |
| *. frontend.clouddatahub.net |443 |HTTPS |
| *. core.windows.net |443 |HTTPS |
| login.microsoftonline.com |443 |HTTPS |
| *. msftncsi.com |443 |Használja a tootest internetkapcsolattal, ha hello átjáró által hello Power BI szolgáltatás nem érhető el. |
| *.microsoftonline-p.com |443 |Konfigurációjától függően a hitelesítéshez használt. |

### <a name="force-https"></a>Az Azure Service Bus HTTPS-kommunikációt kényszerítése
Az Azure Service Bus hello átjáró toocommunicate kényszerítheti a közvetlen TCP; helyett HTTPS használatával azonban ez úgy is jelentősen csökkenti a teljesítményt. Módosíthatja a hello *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* hello érték módosításával fájl `AutoDetect` túl`Https`. Ez a fájl általában *C:\Program Files\On helyszíni adatátjáró*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <a name="faq"></a>Gyakori kérdések

### <a name="general"></a>Általános kérdések

**A Q**: van egy átjáró adatforrások hello felhőben, például az Azure SQL Database? <br/>
**A**: nem. Egy átjáró tooon helyi adatforrások csak csatlakozik.

**A Q**: rendelkezik hello átjáró hello hello adatforrásként azonos számítógépre telepített toobe? <br/>
**A**: nem. hello átjáró toohello az adatforráshoz megadott hello kapcsolat adatainak felhasználásával csatlakozik. Vegye figyelembe a hello átjáró abban az értelemben, ügyfél-alkalmazásként. hello csak regisztrálnia kell az hello funkció tooconnect toohello kiszolgáló nevét a megadott, általában a hello azonos hálózati.

<a name="why-azure-work-school-account"></a>

**A Q**: Miért ne I kell toouse munkahelyi vagy iskolai fiók toosign a? <br/>
**A**: csak használja az Azure munkahelyi vagy iskolai fiókkal, hello helyszíni az átjáró telepítésekor. A bejelentkezési fiók egy Azure Active Directory (Azure AD) által felügyelt bérlői tárolja. Az Azure AD-fiókot egyszerű felhasználónév (UPN) általában hello e-mail cím megfelelő.

**A Q**: a hitelesítő adataimat tároló? <br/>
**A**: hello adatforráshoz beírt hitelesítő adatok titkosítottak és a tárolt hello átjáró Felhőszolgáltatáshoz. hello hitelesítő adatok lesznek visszafejtve hello a helyszíni adatok átjáróra.

**A Q**: vannak-e hálózati sávszélesség vonatkozó követelmények? <br/>
**A**: azt javasoljuk rendelkezik, a hálózati kapcsolat van a jó teljesítmény. Minden környezet más, és elküldött adatok mennyisége hello hello eredményekre van hatással. ExpressRoute használata segíthet a helyszíni és az Azure adatközpontjaiban hello közötti átviteli szintű tooguarantee.
Hello külső eszköz Azure sebesség teszt app toohelp mérőműszer használhatja a teljesítményt.

**A Q**: Mi az az hello késés futó lekérdezések tooa adatforrás hello átjáró? Mi az a legjobb architektúra hello? <br/>
**A**: tooreduce hálózati késés, telepítés hello átjáró lehető Bezárás toohello adatforrásként. Hello tényleges adatforrás hello átjáró telepíthető, ha a közelségi kapcsolat minimálisra hello késés rendszerben jelent meg. Érdemes lehet túl hello adatközpontokban. Például ha a szolgáltatás hello USA nyugati régiója datacenter használ, és egy Azure virtuális Gépen található SQL-kiszolgálóval rendelkezik, az Azure virtuális gép kell hello USA nyugati régiója túl. A közelségi kapcsolat minimálisra csökkenti a késést, és ezzel elkerülheti a kimenő forgalom költségek hello Azure virtuális gép.

**A Q**: hogyan történik az eredmények küldött vissza toohello felhő? <br/>
**A**: eredmények hello Azure Service Bus keresztül zajlik.

**A Q**: vannak-e a bejövő kapcsolatok toohello átjáró hello felhőből? <br/>
**A**: nem. hello átjáró kimenő kapcsolatok tooAzure Service Bus használja.

**A Q**: Mi történik, ha az általam letiltottak kimenő kapcsolatokat? Mire van szükségem az tooopen? <br/>
**A**: hello portok és a gazdagépekhez, amelyek hello átjárót használja.

**A Q**: hello tényleges Windows-szolgáltatás úgynevezett?<br/>
**A**: A szolgáltatások hello átjárót a helyszíni átjáró szolgáltatás neve.

**A Q**: is hello átjáró Windows-szolgáltatás Azure Active Directory-fiókkal való futtatása? <br/>
**A**: nem. Windows-szolgáltatás hello rendelkeznie kell egy érvényes Windows-fiókot. Alapértelmezés szerint hello szolgáltatás SID NT SERVICE\PBIEgwService hello szolgáltatást futtatja.

### <a name="high-availability"></a>Magas rendelkezésre állás és a katasztrófa utáni helyreállítás

**A Q**: milyen beállítások érhetők el a vész-helyreállítási? <br/>
**A**: hello helyreállítási kulcs toorestore használhatja, vagy helyezze át az átjárót. Hello átjáró telepítésekor adja meg a hello helyreállítási kulcsot.

**A Q**: Miért érdemes hello hello helyreállítási kulcs? <br/>
**A**: hello helyreállítási kulcs tartalmaz egy módja toomigrate, vagy az átjáró beállításainak katasztrófa utáni helyreállításhoz.

## <a name="troubleshooting"></a>Hibaelhárítása

**A Q**: hogyan láthatja meg, hogy milyen lekérdezések küldési toohello helyszíni adatforrás? <br/>
**A**: engedélyezheti a lekérdezés nyomon követése, beleértve a küldött hello lekérdezések. Ne felejtse el vissza toohello eredeti érték Miután befejezte a hibakeresési nyomkövetés toochange lekérdezés. A lekérdezés nyomon követése engedélyezve van a Kilépés hoz létre a nagyobb naplókat.

Is megtekintheti, hogy az adatforrás rendelkezik a nyomkövetési lekérdezések eszközök. Például használhatja bővített eseményektől vagy SQL Profiler az SQL Server és az Analysis Services.

**A Q**: Ha az hello átjáró naplói nem? <br/>
**A**: naplók lásd a témakör későbbi részében.

### <a name="update"></a>Frissítés toohello legújabb verziója

Számos problémájának is surface, amikor hello átjáró verziója elavult. Jó általános gyakorlat győződjön meg arról, hogy hello legújabb verzióját használja-e. Ha hello átjáró havonta vagy hosszabb ideig nem frissítette, akkor előfordulhat, hogy hello hello átjáró legújabb verzióját telepítse, és ha Reprodukálja hello probléma.

### <a name="error-failed-tooadd-user-toogroup--2147463168-pbiegwservice-performance-log-users"></a>Hiba: Nem sikerült tooadd felhasználói toogroup. (Felhasználói-2147463168 PBIEgwService teljesítmény)

Ez a hiba kaphat, ha tooinstall hello átjáró tartományvezérlőn, amely nem támogatott. Győződjön meg arról, hogy egy számítógép, amely a tartományvezérlő nem hello-átjáró üzembe helyezéséhez.

## <a name="logs"></a>Naplók

Naplófájlok egy fontos erőforrás hibaelhárítása során.

#### <a name="enterprise-gateway-service-logs"></a>Vállalati átjáró service naplóit

`C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises data gateway\<yyyyymmdd>.<Number>.log`

#### <a name="configuration-logs"></a>Konfigurációs naplók

`C:\Users\<username>\AppData\Local\Microsoft\On-premises data gateway\GatewayConfigurator.log`




#### <a name="event-logs"></a>Eseménynaplók

Hello az adatkezelési átjáró és PowerBIGateway naplók alapján is megtalálhatja **alkalmazás- és szolgáltatásnaplók**.


## <a name="telemetry"></a>Telemetria
Telemetriai adatainak figyelés és hibaelhárítás céljából is használható. Alapértelmezés szerint

**a telemetria tooturn**

1.  Ellenőrizze a hello helyszíni átjáró ügyfél adatkönyvtára hello számítógépen. Általában **%systemdrive%\Program Files\On helyszíni adatátjáró**. Nyissa meg a szolgáltatások konzolt, és ellenőrizze az elérési út tooexecutable hello: hello helyszíni átjáró szolgáltatás tulajdonsága.
2.  Az ügyfél directory hello Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config fájlt. Hello SendTelemetry beállítás tootrue módosítása.
        
    ```
        <setting name="SendTelemetry" serializeAs="String">
                    <value>true</value>
        </setting>
    ```

3.  A módosítások mentéséhez, és indítsa újra a Windows hello-szolgáltatás: A helyszíni átjáró szolgáltatás.




## <a name="next-steps"></a>Következő lépések
* [Analysis Services kezelése](analysis-services-manage.md)
* [Adatok beolvasása az Azure Analysis Services](analysis-services-connect.md)
