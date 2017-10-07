---
title: "aaaHybrid kapcsolatok áttekintése |} Microsoft Docs"
description: "Ez a cikk a hibrid kapcsolatokat, a biztonságot, a TCP-portokat és a támogatott konfigurációkat ismerteti. MABS, WABS."
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: erikre
editor: 
ms.assetid: 216e4927-6863-46e7-aa7c-77fec575c8a6
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/18/2016
ms.author: ccompy
ms.openlocfilehash: f092c6019aae761e1e73f13d1af8446a896515c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hybrid-connections-overview"></a>Hibrid kapcsolatok áttekintése

> [!IMPORTANT]
> A BizTalk Hybrid Connections ki van vezetve, helyét az App Service Hybrid Connections vette át. További információt, beleértve a hogyan toomanage meglévő BizTalk a hibrid kapcsolat: [Azure App Service hibrid kapcsolatok](../app-service/app-service-hybrid-connections.md).

Bevezetés tooHybrid kapcsolatokat, az hello támogatott konfigurációkat, illetve listák hello szükséges TCP-portok.

## <a name="what-is-a-hybrid-connection"></a>Mi a hibrid kapcsolat
A hibrid kapcsolatok az Azure BizTalk Services egyik funkciója. Hibrid kapcsolatok adjon meg egy egyszerű és kényelmesen tooconnect hello Web Apps szolgáltatás az Azure App Service (korábbi nevén webhelyek) és hello Mobile Apps szolgáltatást az Azure App Service (korábban a Mobile Services) tooon helyszíni erőforrások a tűzfal mögött található.

![Hibrid kapcsolatok][HCImage]

A hibrid kapcsolatok előnyei például a következők:

* A Web Apps és a Mobile Apps biztonságosan érheti el a meglévő helyszíni adatokat és szolgáltatásokat.
* Webalkalmazások vagy a Mobile Apps megoszthatja a hibrid kapcsolat tooaccess egy helyszíni erőforrás.
* Minimális TCP-portok szükséges tooaccess vannak a hálózaton.
* Hibrid kapcsolatok használó alkalmazások csak hello adott helyszíni erőforrás közzétett hello hibrid kapcsolat keresztül férnek hozzá.
* A statikus TCP-port, például az SQL Server, a MySQL, a HTTP webes API-k és a legtöbb egyéni webszolgáltatásokat használó tooany helyszíni erőforrás is elérheti.
  
  > [!NOTE]
  > A dinamikus portokat használó TCP-alapú szolgáltatások (például az FTP passzív mód vagy a kiterjesztett paszív mód) jelenleg nem támogatottak. Az LDAP sem támogatott. Az LDAP statikus TCP-portot használ, de UDP-t is használhat. Ennek következtében nem támogatott.
  > 
  > 
* A Web Apps (.NET, PHP, Java, Python, Node.js) és a Mobile Apps (Node.js, .NET) által támogatott összes keretrendszerrel használható.
* Webalkalmazások és Mobile Apps érhetik el a helyszíni erőforrások hello pontosan azonos módon, mintha a helyi hálózaton található hello Web vagy mobilalkalmazás. Például hello ugyanabban a kapcsolati karakterláncban használt helyszíni is használható az Azure-on.

Hibrid kapcsolatok is adja meg a vállalati rendszergazdák és a hibrid alkalmazások, beleértve az elért hello vállalati erőforrás láthatósága:

* Using csoportházirend-beállítások, a rendszergazdák hibrid kapcsolatok engedélyezése hello hálózaton és hibrid alkalmazások által elérhető erőforrásokat is kijelölhet.
* Esemény és naplózási naplók hello a vállalati hálózaton adja meg a hibrid kapcsolatok által elért hello erőforrás láthatósága.

## <a name="example-scenarios"></a>Példaforgatókönyvek
Hibrid kapcsolatok támogatja a következő keretrendszer-alkalmazás kombinációkat hello:

* .NET framework hozzáférés tooSQL kiszolgáló
* .NET framework elérési tooHTTP/HTTPS szolgáltatások WebClient
* A PHP hozzáférés tooSQL kiszolgáló, MySQL
* Java hozzáférés tooSQL Server, a MySQL és Oracle
* A Java elérési tooHTTP/HTTPS-szolgáltatások

Hibrid kapcsolatok tooaccess használatával a helyszíni SQL Server, vegye figyelembe a következő hello:

* Az SQL Express nevű példány konfigurált toouse statikus portokat kell lennie. Alapértelmezés szerint az SQL Express elnevezett példányai dinamikus portokat használnak.
* Az SQL Express alapértelmezett példányai statikus portot használnak, de engedélyezni kell a TCP protokollt. Alapértelmezés szerint a TCP nem engedélyezett.
* Fürtszolgáltatás vagy a rendelkezésre állási csoportok használatakor hello `MultiSubnetFailover=true` mód jelenleg nem támogatott.
* Hello `ApplicationIntent=ReadOnly` jelenleg nem támogatott.
* SQL-hitelesítés kérhető hello Azure-alkalmazást és hello a helyszíni SQL server által támogatott hello végpontok közötti hitelesítési módszerként.

## <a name="security-and-ports"></a>Biztonság és portok
Hibrid kapcsolatok közös hozzáférésű Jogosultságkód (SAS) hitelesítési toosecure hello kapcsolatok használata hello Azure származó alkalmazások és hello helyszíni Hybrid Connection Manager toohello a hibrid kapcsolat. Külön kapcsolaton kulcsok hello alkalmazást hoz létre, hello helyszíni Hybrid Connection Manager. Ezek a kapcsolati kulcsok egymástól függetlenül frissíthetők és visszavonhatók.

Hibrid kapcsolatok nyújtanak hello kulcsok toohello alkalmazások zökkenőmentes és biztonságos terjesztését és hello helyszíni Hybrid Connection Manager.

Lásd: [Create and Manage Hybrid Connections](integration-hybrid-connection-create-manage.md) (Hibrid kapcsolatok létrehozása és felügyelete).

*Alkalmazás engedélyezési elkülönül a hibrid kapcsolat hello*. Bármilyen megfelelő hitelesítési módszer használható. hello engedélyezési módszer hello végpont engedélyezési módszer hello Azure felhőalapú és helyszíni összetevők hello támogatott függ. Az Azure-alkalmazás például egy helyszíni SQL Servert ér el. Ebben az esetben SQL engedélyezési hello hitelesítési módszer, amely támogatott-végpont lehet.

#### <a name="tcp-ports"></a>TCP-portok
A hibrid kapcsolatokhoz csak kimenő TCP- vagy HTTP-kapcsolatra van szükség a magánhálózaton. Nem kell tooopen bármely tűzfalportok, vagy minden bejövő kapcsolat a hálózati szegélyhálózati konfigurációs tooallow váltson át a hálózaton.

a következő TCP-portok hello hibrid kapcsolatok használják:

| Port | Miért szükséges |
| --- | --- |
| 9350 - 9354 |Ezek a portok adatátvitelre szolgálnak. hello Service Bus relay manager vizsgálat port 9350 toodetermine, ha TCP-kapcsolat érhető el. Ha elérhető, akkor feltételezi, hogy a 9352-es port is elérhető. Az adatforgalom a 9352-es porton halad át. <br/><br/>Kimenő kapcsolatok engedélyezése a toothese portok. |
| 5671 |Amikor 9352 portot használja az adatforgalmat, port: 5671 hello vezérlőcsatorna lesz. <br/><br/>Kimenő kapcsolatok engedélyezése a toothis port. |
| 80, 443 |Ezeket a portokat az egyes adatok kérelmek tooAzure használnak. Is, ha a portok 9352 és 5671 nem használható, *majd* 80-as és 443-as portjait hello tartalék adatátvitel és hello vezérlőcsatorna használt portokat.<br/><br/>Kimenő kapcsolatok engedélyezése a toothese portok. <br/><br/>**Megjegyzés:** nem ajánlott toouse ezek helyett hello tartalék portok hello más TCP-portok. az adatok csatornák hello HTTP/WebSocket hello protokoll helyett natív TCP lesz. Ez kisebb teljesítményt eredményezhet. |

## <a name="next-steps"></a>Következő lépések
[Create and Manage Hybrid Connections (Hibrid kapcsolatok létrehozása és felügyelete)](integration-hybrid-connection-create-manage.md)<br/>
[Csatlakozás Azure Web Apps tooan helyszíni erőforrás](../app-service-web/web-sites-hybrid-connection-get-started.md)<br/>
[Csatlakozás tooon helyszíni SQL Server az Azure-webalkalmazás](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)<br/>

## <a name="see-also"></a>Lásd még:
[REST API a BizTalk Services felügyeletéhez a Microsoft Azure-ban](http://msdn.microsoft.com/library/azure/dn232347.aspx)
[BizTalk Services: Kiadások diagramja](biztalk-editions-feature-chart.md)<br/>
[BizTalk-szolgáltatás létrehozása az Azure Portallal](biztalk-provision-services.md)<br/>
[BizTalk Services: Irányítópult, Figyelés és Méret lapok](biztalk-dashboard-monitor-scale-tabs.md)<br/>

[HCImage]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionImage.png
[HybridConnectionTab]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionManageConn.png
