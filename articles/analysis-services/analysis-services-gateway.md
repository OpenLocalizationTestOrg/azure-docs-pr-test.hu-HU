---
title: "A helyszíni adatátjáró |} Microsoft Docs"
description: "Egy helyszíni átjárót szükség, ha az Azure-ban az Analysis Services-kiszolgálóhoz csatlakoznak a helyszíni adatforrások."
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
ms.openlocfilehash: 514b5404e8cbfa0baa657eb41736e20cad502638
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="connecting-to-on-premises-data-sources-with-azure-on-premises-data-gateway"></a>Csatlakozás az adatforrásokhoz helyszíni Azure a helyszíni adatok átjáróval
Az a helyszíni átjáró működik hídként, a helyszíni adatforrások és a felhőben az Azure Analysis Services-kiszolgálók közötti biztonságos adatátvitel biztosítása. Mellett több Azure Analysis Services-kiszolgáló ugyanabban a régióban dolgozik, az átjáró legújabb verzióját is működik az Azure Logic Apps, a Power bi-ban, a kiemelt alkalmazások és a Microsoft Flow. Csak egyetlen átjáró ugyanabban a régióban több szolgáltatáshoz is társíthat. 

 Az Azure Analysis Services egy átjáró-erőforráshoz ugyanabban a régióban van szükség. Például ha Azure Analysis Services-kiszolgálók az USA keleti régiója 2 régióban, kell egy átjáró-erőforráshoz régióban USA keleti régiója 2. USA keleti régiója 2 több kiszolgáló használható ugyanahhoz az átjáróhoz.

Az átjáró telepítése az első alkalommal első az egy négyrészes folyamat:

- **Töltse le és futtassa a telepítőt** – Ez a lépés telepíti egy átjáró szolgáltatás a szervezet egyik számítógépén.

- **Regisztrálnia kell az átjárót** – ebben a lépésben adjon nevet és helyreállítási az átjáró kulcsát, és válasszon ki egy régiót, az átjáró regisztrálása az átjáró Felhőszolgáltatáshoz.

- **Hozzon létre egy átjáró erőforrást az Azure-ban** -ebben a lépésben az Azure-előfizetése létrehozhat egy átjáró-erőforráshoz.

- **Csatlakoztassa a kiszolgálókat az átjáró erőforrás** -előfizetése van egy átjáró-erőforráshoz, amennyiben a kiszolgálók csatlakoztatása megkezdheti.

Ha már van egy átjáró-erőforráshoz az előfizetéséhez beállított, csatlakozhat több kiszolgálót, és egyéb szolgáltatások azt. Csak kell telepíteni egy másik átjárót, és további átjáró erőforrások létrehozása, ha más szolgáltatások vagy kiszolgálók egy másik régióban.

Rögtön használatba, lásd: [telepítse és konfigurálja a helyszíni adatátjáró](analysis-services-gateway-install.md).

## <a name="how-it-works"></a>Annak működéséről
A szervezet egy számítógépre telepítse az átjáró fut a Windows, **helyszíni adatátjáró**. A helyi szolgáltatás az átjáró Felhőszolgáltatáshoz Azure Service Buson keresztül van regisztrálva. Ezután hozzon létre egy átjáró-erőforráshoz átjáró Felhőszolgáltatáshoz Azure-előfizetése. Az Azure Analysis Services-kiszolgáló majd csatlakoznak az átjáró-erőforráshoz. Ha a kiszolgálón lévő modellek az adatokhoz történő kapcsolódáshoz a helyszíni adatforrások lekérdezések és feldolgozásra vonatkozó, a lekérdezés és az adatok áramlását az átjáró-erőforráshoz, Azure Service Bus, a helyi helyszíni átjáró szolgáltatás és az adatforrások halad át. 

![Működés](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Lekérdezések és adatfolyam:

1. A lekérdezés a felhőalapú szolgáltatás, a titkosított hitelesítő adatokkal a helyszíni adatforrás hozta létre. A várólista feldolgozása az átjáró majd érkezik.
2. Az átjáró felhőszolgáltatáshoz elemzi a lekérdezést, és leküldéses értesítések a kérést a [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).
3. Az a helyszíni átjáró kérdezze le az Azure Service Bus, a függőben lévő kérelmek.
4. Az átjáró a lekérdezés lekérdezi, visszafejti a hitelesítő adatokat, és ezeket a hitelesítő adatokat az adatforrások kapcsolódik.
5. Az átjáró a lekérdezést küld az adatforrás-végrehajtásra.
6. Az eredményeket az adatforrásból kerülnek vissza az átjáró, majd a felhőszolgáltatás és a kiszolgáló.

## <a name="windows-service-account"></a>Windows-szolgáltatás fiókja
Az a helyszíni átjáró használatára van konfigurálva *NT SERVICE\PBIEgwService* a Windows szolgáltatás bejelentkezési hitelesítő adatok. Alapértelmezés szerint rendelkezik bejelentkezési jobb szolgáltatásként; a gépet, telepíti az átjárót a környezetében. Ezeket a hitelesítő adatokat, de nem ugyanazt a fiókot a helyszíni adatforrások eléréséhez használt az Azure-fiókjával.  

Ha problémák lépnek fel a proxykiszolgáló hitelesítést miatt, előfordulhat, hogy módosítani szeretné a Windows-fiók egy tartományi felhasználó vagy a felügyelt szolgáltatásfiók.

## <a name="ports"></a>Portok
Az átjáró Azure Service Bus egy kimenő kapcsolatot hoz létre. A kimenő portokon kommunikál: TCP 443-as (alapértelmezett), 5671, 5672, 9350 – 9354-es.  Az átjáró nincs szükség a bejövő portra.

Javasoljuk, hogy a tűzfal az adatterület az IP-címek engedélyezési listája. Letöltheti a [Microsoft Azure Datacenter IP-lista](https://www.microsoft.com/download/details.aspx?id=41653). A lista a heti frissül.

> [!NOTE]
> Az IP-címek szerepel-e az Azure Datacenter IP CIDR-formátumban vannak. Például: 10.0.0.0/24 nem jelenti azt 10.0.0.0 10.0.0.24 keresztül. További információ a [CIDR-jelöléssel](http://whatismyipaddress.com/cidr).
>
>

Az alábbiakban a teljes tartománynevek az átjáró által használt.

| Tartománynevek | Kimenő portok | Leírás |
| --- | --- | --- |
| *. powerbi.com webhelyre |80 |A telepítő letöltéséhez használt HTTP. |
| *. powerbi.com webhelyre |443 |HTTPS |
| *. analysis.windows.net |443 |HTTPS |
| *. login.windows.net |443 |HTTPS |
| *. servicebus.windows.net |5671-5672 |Speciális üzenetsor-kezelési protokoll (AMQP) |
| *. servicebus.windows.net |443, 9350-9354 |A Service Bus Relay (a 443-as kér a hozzáférés-vezérlés jogkivonat beszerzése) TCP-n keresztül figyelői |
| *. frontend.clouddatahub.net |443 |HTTPS |
| *. core.windows.net |443 |HTTPS |
| login.microsoftonline.com |443 |HTTPS |
| *. msftncsi.com |443 |Használja az internet kapcsolat tesztelése, ha az átjáró a Power BI szolgáltatás nem érhető el. |
| *.microsoftonline-p.com |443 |Konfigurációjától függően a hitelesítéshez használt. |

### <a name="force-https"></a>Az Azure Service Bus HTTPS-kommunikációt kényszerítése
Beállíthatja, hogy az közvetlen TCP; helyett HTTPS használatával kommunikálnak Azure Service Bus-átjáró azonban ez úgy is jelentősen csökkenti a teljesítményt. Módosíthatja a *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* megváltoztatja az értéket a fájl `AutoDetect` való `Https`. Ez a fájl általában *C:\Program Files\On helyszíni adatátjáró*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <a name="faq"></a>Gyakori kérdések

### <a name="general"></a>Általános kérdések

**A Q**: van egy átjáró adatforrások, például az Azure SQL-adatbázis a felhőben? <br/>
**A**: nem. Átjáró csatlakozik a helyi adatforrások csak.

**A Q**: az adatforrással azonos gépen kell telepíteni az átjárót rendelkezik? <br/>
**A**: nem. Az átjáró kapcsolódik az adatforráshoz, a megadott kapcsolati információk használatával. Egy ügyfélalkalmazás abban az értelemben, fontolja meg az átjáró. Az átjáró éppen kell képes csatlakozni a kiszolgáló neve, a megadott, általában ugyanazon a hálózaton.

<a name="why-azure-work-school-account"></a>

**A Q**: Miért szükséges a munkahelyi vagy iskolai fiókkal való bejelentkezéshez használt? <br/>
**A**: csak használja az Azure munkahelyi vagy iskolai fiókkal, az a helyszíni átjáró telepítésekor. A bejelentkezési fiók egy Azure Active Directory (Azure AD) által felügyelt bérlői tárolja. Általában az Azure AD-fiókot egyszerű felhasználónév (UPN) felel meg az e-mail cím.

**A Q**: a hitelesítő adataimat tároló? <br/>
**A**: adatforráshoz megadott hitelesítő adatok titkosítva, és az átjáró felhőszolgáltatásban tárolt. A hitelesítő adatokat a helyszíni adatok átjáróra lesznek visszafejtve.

**A Q**: vannak-e hálózati sávszélesség vonatkozó követelmények? <br/>
**A**: azt javasoljuk rendelkezik, a hálózati kapcsolat van a jó teljesítmény. Minden környezet más, és a küldött adatok mennyisége hatással van az eredményeket. A helyszíni és az Azure adatközpontjaiban közötti átviteli szintű segíthet az ExpressRoute segítségével.
A külső eszköz Azure sebesség teszt alkalmazás segítségével fel tudja mérni a teljesítményt.

**A Q**: Mi az a várakozási lekérdezések futtatását adatforráshoz az átjáró? Mi az a legjobb architektúra? <br/>
**A**: hálózati késés csökkentésére, telepítse az átjáró lehető az adatforrás a lehető legjobban hasonlítson. Az átjáró telepíthető a konkrét adatforrásokhoz, ha a közelségi kapcsolat minimalizálja a várakozási bevezetett. Túl vegye figyelembe az adatközpontokban. Például ha a szolgáltatás használja, az USA nyugati régiója adatközpontban, és egy Azure virtuális Gépen található SQL-kiszolgálóval rendelkezik, az Azure virtuális gép kell lennie az USA nyugati régiója túl. A közelségi kapcsolat minimálisra csökkenti a késést, és ezzel elkerülheti a kimenő forgalom díjak, az Azure virtuális gépen.

**A Q**: hogyan eredmények küldött vissza a felhőben? <br/>
**A**: eredmények az Azure Service Bus keresztül zajlik.

**A Q**: vannak-e a bejövő kapcsolatokat az átjáróra a felhőből? <br/>
**A**: nem. Az átjáró Azure Service Bus kimenő kapcsolatokat használja.

**A Q**: Mi történik, ha az általam letiltottak kimenő kapcsolatokat? Mit kell megnyitni? <br/>
**A**: a portok és a gazdagépekhez, amelyek az átjárót használja.

**A Q**: a tényleges Windows-szolgáltatás úgynevezett?<br/>
**A**: A szolgáltatások, az átjárót a helyszíni átjáró szolgáltatás neve.

**A Q**: Azure Active Directory-fiókkal az átjáró Windows-szolgáltatás futtatható? <br/>
**A**: nem. A Windows-szolgáltatás egy érvényes Windows-fiókkal kell rendelkeznie. Alapértelmezés szerint a szolgáltatás fut a szolgáltatás SID NT SERVICE\PBIEgwService.

### <a name="high-availability"></a>Magas rendelkezésre állás és a katasztrófa utáni helyreállítás

**A Q**: milyen beállítások érhetők el a vész-helyreállítási? <br/>
**A**: a helyreállítási kulcs használatával állítsa vissza, vagy helyezze át az átjárót. Az átjáró telepítésekor adja meg a helyreállítási kulcsot.

**A Q**: Mi az az előnye, hogy a helyreállítási kulcs? <br/>
**A**: A helyreállítási kulcs biztosítja az áttelepítéshez, vagy az átjáró beállításainak katasztrófa utáni helyreállításhoz.

## <a name="troubleshooting"></a>Hibaelhárítása

**A Q**: Hogyan tekinthető meg mi lekérdezések folyamatban van a helyszíni adatforrás küldött? <br/>
**A**: engedélyezheti a lekérdezés nyomon követése, beleértve a küldött lekérdezések. Ne felejtse el módosítani a lekérdezés vissza az eredeti értéket, miután befejezte a hibakeresési nyomkövetés. A lekérdezés nyomon követése engedélyezve van a Kilépés hoz létre a nagyobb naplókat.

Is megtekintheti, hogy az adatforrás rendelkezik a nyomkövetési lekérdezések eszközök. Például használhatja bővített eseményektől vagy SQL Profiler az SQL Server és az Analysis Services.

**A Q**: hol találhatók az átjáró naplói? <br/>
**A**: naplók lásd a témakör későbbi részében.

### <a name="update"></a>Frissítse a legújabb verzióra

Számos problémájának is surface, ha az átjáró verziója elavult. Jó általános gyakorlat győződjön meg arról, hogy a legújabb verzióját használja-e. Ha az átjáró nem frissültek, havonta vagy hosszabb ideig, akkor előfordulhat, hogy javasoljuk, hogy az átjáró legújabb verzióját telepítse, és ha Reprodukálja a hibát.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a>Hiba: Nem sikerült hozzáadni a felhasználót a csoporthoz. (Felhasználói-2147463168 PBIEgwService teljesítmény)

Ez a hiba kaphat, ha az átjáró telepítésekor a tartományvezérlőn, amely nem támogatott. Győződjön meg arról, hogy telepít-e az átjáró gépen, amely nem tartományvezérlő.

## <a name="logs"></a>Naplók

Naplófájlok egy fontos erőforrás hibaelhárítása során.

#### <a name="enterprise-gateway-service-logs"></a>Vállalati átjáró service naplóit

`C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises data gateway\<yyyyymmdd>.<Number>.log`

#### <a name="configuration-logs"></a>Konfigurációs naplók

`C:\Users\<username>\AppData\Local\Microsoft\On-premises data gateway\GatewayConfigurator.log`




#### <a name="event-logs"></a>Eseménynaplók

Az adatkezelési átjáró, a PowerBIGateway naplókat alatt található **alkalmazás- és szolgáltatásnaplók**.


## <a name="telemetry"></a>Telemetria
Telemetriai adatainak figyelés és hibaelhárítás céljából is használható. Alapértelmezés szerint

**Telemetria bekapcsolása**

1.  Ellenőrizze a helyszíni adatok átjáró ügyfél könyvtárat a számítógépen. Általában **%systemdrive%\Program Files\On helyszíni adatátjáró**. Nyissa meg a szolgáltatások konzolt, és ellenőrizze az elérési utat a végrehajtható fájl: A tulajdonság a helyszíni adatok átjáró szolgáltatás.
2.  Az ügyfél könyvtárból Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config fájlban. Módosítsa a SendTelemetry beállítást igaz értékre.
        
    ```
        <setting name="SendTelemetry" serializeAs="String">
                    <value>true</value>
        </setting>
    ```

3.  A módosítások mentéséhez, és indítsa újra a Windows-szolgáltatás: A helyszíni átjáró szolgáltatás.




## <a name="next-steps"></a>Következő lépések
* [Analysis Services kezelése](analysis-services-manage.md)
* [Adatok beolvasása az Azure Analysis Services](analysis-services-connect.md)
