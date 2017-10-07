---
title: AAA "az Azure App Service hibrid kapcsolatok |} Microsoft dokumentumok"
description: "Hogyan toocreate és -felhasználási hibrid kapcsolatok tooaccess erőforrások különálló hálózaton"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/22/2017
ms.author: ccompy
ms.openlocfilehash: 61d58068ab0a7c803019e3f0e92bde4273d1a053
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-hybrid-connections"></a>Az Azure App Service hibrid kapcsolatok #

## <a name="overview"></a>Áttekintés ##

Hibrid kapcsolatok, mind az Azure-szolgáltatás, valamint a hello Azure App Service egy szolgáltatása.  Szolgáltatásként rendelkezik használatát és képességekkel ruházzák, hogy vannak-e kihasználhatók a hello Azure App Service szolgáltatásban.  Hibrid kapcsolatok és hello Azure App Service itt kezdheti kívül használatát további toolearn [Azure hibrid kapcsolatok][HCService]

Belül hello Azure App Service a hibrid kapcsolatok lehet más hálózatokon használt tooaccess alkalmazás-erőforrásokat. Az alkalmazás tooan alkalmazás végpontjának hozzáférést biztosít.  Nem engedélyezi az alternatív funkció tooaccess az alkalmazást.  Az App Service hello alkalmazott, minden egyes hibrid kapcsolat tooa egyetlen TCP-állomás és port kombinációja ad eredményül.  Ez azt jelenti, hogy hello hibrid kapcsolat végpont operációs rendszert és minden alkalmazás biztosítható, hogy Ön elérte a TCP-figyelési port-e, hogy. Hibrid kapcsolatok nem tudja, vagy attól, milyen hello protokoll, vagy mi elérésére.  Hálózati hozzáférés egyszerűen is ellát.  


## <a name="how-it-works"></a>Működés ##
hello hibrid kapcsolatok szolgáltatás két kimenő hívások tooService Bus Relay foglalja magában.  Van-e kapcsolat a hello állomáson, ahol az alkalmazás az hello app service fut, és majd van-e kapcsolat a hibrid kapcsolat Manager(HCM) tooService Bus-továbbító hello tárból.  hello HCM szolgáltatás hello hálózati üzemeltető belül üzembe helyezett továbbítási szolgáltatás 

Hello keresztül az alkalmazás rendelkezik egy TCP-alagút tooa két illesztett kapcsolatok hello HCM másik oldalán rögzített hello állomás: port kombinációjához.  hello kapcsolat használja a TLS 1.2 biztonsági és a SAS-kulcs a hitelesítési/engedélyezési.    

![][1]

Ha az alkalmazás egy DNS-kérelmet küld, amely megfelel a konfigurálása a hibrid kapcsolat végpont, majd hello kimenő TCP-forgalmat a rendszer átirányítja hello hibrid kapcsolat le.  

> [!NOTE]
> Ez azt jelenti, hogy meg kell tooalways használja a hibrid kapcsolat egy DNS-neve.  Néhány ügyfélszoftver nem teendő a DNS-címkeresést hello végpont helyette használja az IP-címet.
>
>

A hibrid kapcsolatok, hello új hibrid kapcsolatok, amely az Azure-továbbító szolgáltatás felajánlott és a régebbi BizTalk hibrid kapcsolatok hello két típusa van.  hello régebbi BizTalk hibrid kapcsolatok hivatkozott tooas klasszikus hibrid kapcsolatok hello portálon.  Nincs további információ a jelen dokumentum azokat.

### <a name="app-service-hybrid-connection-benefits"></a>App Service hibrid kapcsolat előnyei ###

Számos előnyt toohello hibrid kapcsolatok funkció is

- Alkalmazások biztonságosan férjenek hozzá a helyi rendszeren és a szolgáltatások biztonságosan
- hello szolgáltatást nem szükséges az interneten elérhető végponton
- gyors és egyszerű tooset mentése  
- minden egyes hibrid kapcsolat kiváló biztonsági eleme tooa egyetlen állomás: port kombináció megegyezik
- általában nem szükséges tűzfal lyuk hello kapcsolatok vannak az összes kimenő szokásos webes portokon keresztül
- mivel hello szolgáltatás is azt jelenti, hogy az alkalmazás- és hello technológia hello végpont által használt független toohello nyelvét hálózati szint
- Ez lehet az egyetlen alkalmazást több hálózatokon használt tooprovide hozzáférés 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Művelet nem hajtható végre a hibrid kapcsolatok ###

Néhány dolgot hibrid kapcsolatokkal nem hajtható végre, és tartalmazzák:

- a meghajtó csatlakoztatása
- UDP segítségével
- fér hozzá a TCP-alapú szolgáltatásokat, például a passzív FTP-módban, vagy passzív módban bővített dinamikus portok használatára
- LDAP-támogatás, ahogy néha szükséges UDP
- Active Directory-támogatás

## <a name="adding-and-creating-a-hybrid-connection-in-your-app"></a>Hozzáadása és az alkalmazás egy hibrid kapcsolat létrehozása ##

Hibrid kapcsolatok is létrehozható, hello app portálon keresztül vagy hello hibrid kapcsolat szolgáltatás portálján.  Erősen ajánlott hello app portál toocreate hello hibrid kapcsolatok toouse az alkalmazással szeretné használni.  Nyissa meg a hibrid kapcsolat toocreate toohello [Azure-portálon] [ portal] és az alkalmazás felhasználói felületén hello kísérhet.  Válassza ki **hálózati > hibrid kapcsolati végpontok konfigurálása**.  Itt látható az alkalmazás használatára konfigurált hello hibrid kapcsolatok  

![][2]

új hibrid kapcsolat tooadd kattintson a hibrid kapcsolat hozzáadása.  felhasználói felület, a megnyitott hello hello hibrid kapcsolatok már létrehozott sorolja fel.  egy vagy több azok tooadd tooyour alkalmazást, és kattintson a hello állók közül. szeretné, majd nyomja le **hozzáadása a hibrid kapcsolat kijelölt**.  

![][3]

Ha azt szeretné, hogy az új hibrid kapcsolat toocreate, kattintson a **új hibrid kapcsolat létrehozása**.  Itt adja meg a: 

- a végpont neve
- végpont állomásneve
- Végpont portja
- a szolgáltatásbusz-névtér kívánja toouse

![][4]

Minden hibrid kapcsolat kapcsolt tooa service bus-névtér és minden egyes service bus-névtér Azure-régióban.  Fontos tootry és használja a service bus a névtér hello és az alkalmazás ugyanabban a régióban tooavoid előidézett hálózati késés, így.

Ha azt szeretné, tooremove a hibrid kapcsolat az alkalmazásból, a jobb gombbal kattintson rá, és válassza ki **Disconnect**.  

A hibrid kapcsolat tooyour webalkalmazás hozzáadása után részletes információkat tekinthet meg egyszerűen kattintással.  

![][5]

## <a name="hybrid-connections-and-app-service-plans"></a>Hibrid kapcsolatok és az App Service-csomagokról ##

hajtsa végre a hello csak hibrid kapcsolatok hello új hibrid kapcsolatok.  Csak azok a Basic, Standard, Premium és termékváltozatok árképzési elszigetelt érhető el.  Nincsenek kötött korlátok toohello árképzési terv.  

| Terv díjszabása | Használható hello terv a hibrid kapcsolatok száma |
|----|----|
| Basic | 5 |
| Standard | 25 |
| Prémium | 200 |
| Izolált | 200 |

Mivel nincsenek App Service-csomag korlátozások is van felhasználói felület hello App Service-csomag, amely bemutatja, hogy hány hibrid kapcsolatok használatban vannak, és milyen alkalmazások.  

![][6]

Ahogy hello app nézet, részletes információkat tekinthet a hibrid kapcsolat azt.  Itt látható: hello app nézet kellett összes hello információkat talál, de is láthatja, hogy hány más alkalmazások hello hello tulajdonságai azonos App Service-csomag használ, hogy a hibrid kapcsolat.

![][7]

Míg egy hibrid kapcsolati végpontok, amely egy App Service-csomag használható hello számára vonatkozó korlátozást, minden egyes használt hibrid kapcsolat az adott App Service-csomag alkalmazások tetszőleges számú segítségével.  Ez az, hogy ha 1 hibrid kapcsolat az App Service-csomag az 5 külön alkalmazásokban használt, ez továbbra is csak 1 hibrid kapcsolat toosay.

Egy további költség nélkül toohybrid kapcsolatok alatt csak a Basic, Standard, Premium vagy elkülönített SKU felhasználható túl van.  A hibrid kapcsolat árakkal kapcsolatos részletekért nyissa meg itt: [Service Bus árképzési][sbpricing].

## <a name="hybrid-connection-manager"></a>Hibrid Csatlakozáskezelő ##

Ahhoz, hogy hibrid kapcsolatok toowork van szüksége, amelyen a hibrid kapcsolat végpont hello hálózat továbbító ügynök.  A továbbító ügynök hello hibrid kapcsolat Manager (HCM) neve.  Ez az eszköz letölthető a hello **hálózati > hibrid kapcsolati végpontok konfigurálása** felhasználói felület érhető el az alkalmazást a hello [Azure-portálon][portal].  

Ez az eszköz fut, a Windows server 2008 R2 és a Windows.  Miután telepített hello HCM szolgáltatásként.  Ez a szolgáltatás tooAzure szolgáltatásbusz továbbítási konfigurált hello végpontok alapján csatlakozik.  hello HCM hello kapcsolódásának 80-as és 443-as kimenő tooports.    

hello HCM rendelkezik a felhasználói felület tooconfigure azt.  Hello HCM telepítése után van lehetősége hello felhasználói felület, amely az HybridConnectionManagerUi.exe hello futtatásával hello Hybrid Connection Manager telepítési könyvtárában.  Azt is könnyen elérésekor a Windows 10 beírásával *Hybrid Connection Manager felhasználói felületén* be a keresőmezőbe.  

HCM felhasználói felületének indítása hello hello először thing, lásd: esetén hello hibrid kapcsolatok hello HCM ezen példányának használatára konfigurált összes tábla.  Ha toomake szüksége lesz az Azure-ral tooauthenticate módosításokat. 

tooadd egy vagy több hibrid kapcsolatok tooyour HCM:

1. Hello HCM felhasználói felületének indítása
1. Kattintson egy másik hibrid kapcsolat konfigurálása![][8]

1. Jelentkezzen be az Azure-fiókjával
1. Előfizetés kiválasztása
1. Kattintson a HCM toorelay kívánt hello hibrid kapcsolatok![][9]

1. Kattintson a Save (Mentés) gombra.

Ezen a ponton hello hibrid kapcsolatok hozzáadott megjelenik.  Kattintson a hello konfigurálva hibrid kapcsolat is, és hello kapcsolat adatainak megtekintése.

![][10]

HCM toobe képes toosupport hello hibrid kapcsolat van konfigurálva a szükséges:

- TCP hozzáférési tooAzure 80-as és 443-as porton keresztül
- TCP-hozzáférés toohello hibrid kapcsolat végpont
- képes toodo DNS keresések hello végpont állomás és hello azure szolgáltatásbusz-névtér

hello HCM támogatja az új hibrid kapcsolatok, valamint hello régebbi BizTalk hibrid kapcsolatok.

### <a name="redundancy"></a>Redundancia ###

Minden egyes HCM támogathatja több hibrid kapcsolatok.  Ezenkívül egy adott hibrid kapcsolathoz több HCMs használható.  hello alapértelmezett működése tooround időszeletelési forgalom keresztül hello HCMs konfigurált bármely adott végpont.  Ha magas rendelkezésre állás a hibrid kapcsolatok használata esetén a hálózatról, egyszerűen hozható létre több HCMs külön gépeken. 

### <a name="manually-adding-a-hybrid-connection"></a>Hibrid kapcsolat manuális hozzáadása ###

Ha valaki az előfizetés toohost kívül egy adott a hibrid kapcsolat HCM példánya, megoszthatja őket hello hello a hibrid kapcsolat átjáró kapcsolati karakterlánca.  Ez hello a hibrid kapcsolat tulajdonságainak hello látható [Azure-portálon][portal]. karakterlánc, amely toouse kattintson hello **kézzel konfigurálásához** hello HCM gombra, és illessze be a hello átjáró kapcsolati karakterlánca.


## <a name="troubleshooting"></a>Hibaelhárítás ##

Ha egy futó alkalmazáshoz be van állítva a a hibrid kapcsolat legalább egy HCM, amely rendelkezik a hibrid kapcsolat konfigurálva van, majd hello állapot megtudhatja, hogy **csatlakoztatva** hello portálon.  Ha ezt nem kéri a **csatlakoztatva** az azt jelenti, hogy vagy az alkalmazás nem működik, vagy a HCM out tooAzure 80-as vagy 443-as porton nem tud csatlakozni.  

hello elsődleges ok, hogy az ügyfelek tootheir végpont nem lehet csatlakozni az oka, hogy hello végpont megadott helyett egy DNS-nevet egy IP-címet használja.  Ha az alkalmazás nem érhető el a szükséges hello végpontot, és egy IP-címet, váltson toousing egy hello HCM futtató hello gazdagépen érvényes DNS-név.  Más dolgokat toocheck, ahol hello HCM fut, valamint, hogy van-e kapcsolat hello HCM futtató toohello hibrid kapcsolat végpont hello gazdagépről hello gazdagépen megfelelően feloldja adott hello DNS-nevét.  

Nincs olyan eszköz hello alkalmazásszolgáltatás, mely tcpping nevű hello konzolról is elindítható.  Ez az eszköz segítségével megállapíthatja, hogy ha a hozzáférési tooa TCP-végpont rendelkezik, de nem közli, hogy van-e hozzáférési tooa hibrid kapcsolat végpont.  A hibrid kapcsolat végpontjának elleni hello konzol használatakor a sikeres ping csak jelzi, hogy rendelkezik-e konfigurálva az alkalmazáshoz, a gazdagép és a portszám kombináció használó hibrid kapcsolat.  

## <a name="biztalk-hybrid-connections"></a>Hibrid kapcsolatok a BizTalk szolgáltatásokban ##

hello régebbi BizTalk hibrid kapcsolatok funkció kikapcsolása toofurther BizTalk a hibrid kapcsolat létrehozása be lett zárva.  A már meglévő BizTalk hibrid kapcsolatok használata az alkalmazások továbbra is, de toohello új szolgáltatást kell telepítenie.  Hello között hello új szolgáltatás hello BizTalk verzióra előnye van:

- Nincsenek további BizTalk fiókot kell megadni
- A TLS 1.2-es 1.0 hasonlóan BizTalk hibrid kapcsolatok helyett
- Kommunikációs 80-as és a DNS neve tooreach Azure helyett IP-címek és további számos 443-as porton keresztül más portok.  

tooadd a BizTalk hibrid kapcsolat tooyour alkalmazások hello lépjen tooyour alkalmazás [Azure-portálon] [ portal] kattintson **hálózati > hibrid kapcsolati végpontok konfigurálása**.  Hello klasszikus hibrid kapcsolatok tábla kattintson **klasszikus hibrid kapcsolat hozzáadása a**.  Itt látni fogja a BizTalk hibrid kapcsolatok listáját.  


<!--Image references-->
[1]: ./media/app-service-hybrid-connections/hybridconn-connectiondiagram.png
[2]: ./media/app-service-hybrid-connections/hybridconn-portal.png
[3]: ./media/app-service-hybrid-connections/hybridconn-addhc.png
[4]: ./media/app-service-hybrid-connections/hybridconn-createhc.png
[5]: ./media/app-service-hybrid-connections/hybridconn-properties.png
[6]: ./media/app-service-hybrid-connections/hybridconn-aspproperties.png
[7]: ./media/app-service-hybrid-connections/hybridconn-hcm.png
[8]: ./media/app-service-hybrid-connections/hybridconn-hcmadd.png
[9]: ./media/app-service-hybrid-connections/hybridconn-hcmadded.png
[10]: ./media/app-service-hybrid-connections/hybridconn-hcmdetails.png

<!--Links-->
[HCService]: http://docs.microsoft.com/azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: http://portal.azure.com/
[oldhc]: http://docs.microsoft.com/azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: http://azure.microsoft.com/pricing/details/service-bus/

