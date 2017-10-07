---
title: "aaaAzure DMZ példa – Build az NSG-ket egy egyszerű DMZ |} Microsoft Docs"
description: "A hálózati biztonsági csoportokkal (NSG) DMZ összeállítása"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 11c5c6026da30fbc9c5e585f5c16e2d411d6fd80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-an-azure-resource-manager-template"></a>1 – például egy egyszerű DMZ NSG-k használata az Azure Resource Manager-sablon létrehozása
[Visszatérési toohello biztonsági határ bevált gyakorlatok lap][HOME]

> [!div class="op_single_selector"]
> * [Resource Manager-sablon](virtual-networks-dmz-nsg.md)
> * [Klasszikus - PowerShell](virtual-networks-dmz-nsg-asm.md)
> 
>

Ebben a példában egy egyszerű DMZ négy Windows-kiszolgálók és hálózati biztonsági csoportokat hoz létre. Ez a példa bemutatja az egyes hello megfelelő sablont szakaszok tooprovide bemutatják az egyes lépéseket. Nincs is a forgalom forgatókönyv szakasz tooprovide egy hogyan forgalom halad keresztül hello hello DMZ a védelmi részletesebb lépésenkénti tekintse meg. Végezetül hello references szakaszában van hello teljes sablon kód és utasítások toobuild a környezet tootest és a különböző forgatókönyvekben a kísérletet. 

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] 

![Az NSG bejövő DMZ][1]

## <a name="environment-description"></a>Környezet leírása
Ebben a példában egy előfizetés tartalmazza a következő erőforrások hello:

* Egyetlen erőforráscsoportként működnek
* Egy virtuális hálózatot, két alhálózattal; "Előtér" és "Háttér"
* A hálózati biztonsági csoport, amely alkalmazott tooboth alhálózatok
* A Windows Server egy webalkalmazás-kiszolgáló ("IIS01") jelölő
* Két windows Server kiszolgálókon, amelyek megfelelnek az alkalmazás háttérkiszolgálók ("AppVM01", "AppVM02")
* A Windows server DNS-kiszolgáló ("DNS01") jelölő
* A nyilvános IP-cím hello alkalmazás webkiszolgáló társított

Hello references szakaszában nincs hivatkozás tooan Azure Resource Manager sablon hello környezet ebben a példában leírt épülő. Épület hello virtuális gépek és virtuális hálózatok, bár hello példa-sablon által nem ismerteti a jelen dokumentum részletesen. 

**toobuild ebben a környezetben** (részletes utasításokat amelyek hello references szakaszában, a jelen dokumentum);

1. Telepítése Azure Resource Manager sablon hello: [Azure gyors üzembe helyezés sablonok][Template]
2. Hello minta alkalmazást telepíteni: [minta alkalmazás-parancsfájl][SampleApp]

>[!NOTE]
>tooRDP tooany háttérkiszolgálók ebben a példában hello IIS-kiszolgálón használt egy "ugrást mező." Első RDP toohello IIS-kiszolgálót és majd hello IIS Server RDP toohello háttér-kiszolgálóról. Másik lehetőség egy nyilvános IP-cím társítható minden kiszolgáló egyszerűbb az RDP a hálózati Adapternek.
> 
>

hello következő szakaszokban hello hálózati biztonsági csoport, és hogyan működik ez a példa az útmutató alapján kulcs sornyi hello Azure Resource Manager-sablon által részletes leírását.

## <a name="network-security-groups-nsg"></a>Hálózati biztonsági csoportok (NSG)
Az ebben a példában egy NSG-csoport összeállítása és hat szabályokkal majd betölteni. 

>[!TIP]
>Általánosságban véve kell először hozza létre az adott "Engedélyezés" szabályokat, és majd hello utolsó általánosabbá "Deny" szabályokat. hello prioritású határozzák meg, hogy mely szabályokat értékeli ki a rendszer először. Forgalom található tooapply tooa meghatározott szabály, ha nincsenek további szabályok kiértékelése. NSG-szabályok alkalmazhat vagy hello bejövő vagy kimenő irányban (szempontjából hello hello alhálózati).
>
>

A következő szabályok hello deklaratív módon, a bejövő forgalom alatt beépített:

1. Belső DNS-forgalom (53-as port) engedélyezve van
2. RDP-forgalom (3389-es port) a hello Internet tooany VM engedélyezve van
3. HTTP-forgalom (80-as port) hello internetes tooweb kiszolgáló (IIS01) engedélyezve van
4. Engedélyezett IIS01 tooAppVM1 minden forgalmat (minden porthoz)
5. Teljes virtuális hálózatot (alhálózatok mindkét) megtagadva hello Internet toohello minden forgalmat (minden porthoz)
6. Minden forgalmat (minden porthoz) hello Frontend alhálózathoz toohello Backend alhálózathoz megtagadva

E szabályok kötött tooeach alhálózattal, ha a HTTP-kérelem nem bejövő webkiszolgálóról hello Internet toohello, mindkét szabályok 3 (engedélyezése) és 5 (megtagadni) szeretné alkalmazni, de mivel a szabály 3 egy magasabb prioritással bír, csak azt csak akkor vonatkozik, és 5 szabály nem lesz play kerülhet. Így hello HTTP-kérelem volna engedélyezett toohello webkiszolgáló. Ha ugyanaz a forgalom próbált tooreach hello DNS01 server, a szabály 5 (Megtagadás) lenne hello első tooapply és hello forgalom volna nem engedélyezett toopass toohello kiszolgáló. 6 (Megtagadás) szabály blokkolja hello Frontend alhálózathoz a toohello Backend alhálózathoz (kivéve a engedélyezett forgalom szabályokban 1 és 4) van szó, a szabálykészlet hello háttér hálózathoz védi, abban az esetben, ha egy támadó biztonság sérüléseinek hello webalkalmazás hello előtér, hello támadó volna korlátozott hozzáférés toohello háttérbeli "védett" hálózati (csak hello AppVM01 kiszolgálón kitett tooresources).

Nincs olyan alapértelmezett kimenő szabály, amely lehetővé teszi, hogy a toohello kimenő forgalom internet. Ebben a példában a Microsoft most átengedi a kimenő forgalmat, és nem módosítja az egyetlen kimenő szabályok. tooapply biztonsági házirend tootraffic mindkét irányban, felhasználói definiált útválasztás szükség, és a"3" hello a felfedezte van [biztonsági határ bevált gyakorlatok lap][HOME].

Minden egyes szabály az alábbiak szerint tárgyalja részletesen:

1. A hálózati biztonsági csoport erőforrás példányként létrehozott toohold hello szabályokat kell lennie:

    ```JSON
    "resources": [
      {
        "apiVersion": "2015-05-01-preview",
        "type": "Microsoft.Network/networkSecurityGroups",
        "name": "[variables('NSGName')]",
        "location": "[resourceGroup().location]",
        "properties": { }
      }
    ]
    ``` 

2. Ebben a példában a hello első szabály lehetővé teszi, hogy a DNS-forgalom hello backend alhálózathoz összes belső hálózatok toohello DNS-kiszolgáló között. hello szabály néhány fontos paraméterekkel rendelkezik:
  * "destinationAddressPrefix" - szabályok használhatja az "alapértelmezett címke" nevű címelőtag különleges típusú, és ezekkel a címkékkel rendszer által biztosított azonosítók, amelyek lehetővé teszik egy egyszerű módot tooaddress címelőtagokat nagyobb kategóriáját. Ez a szabály által használt alapértelmezett címke "Internet" hello toosignify bármely cím hello virtuális hálózaton kívül. Más előtag feliratai VirtualNetwork és AzureLoadBalancer.
  * "Irány" azt jelzi, hogy a forgalom iránya a szabály lép érvénybe. hello használata hello szempontjából hello alhálózati vagy a virtuális gép (attól függően, ahol az NSG kötve van). Így ha irány "Bejövő" és a forgalom hello alhálózati írja be, hello szabályt alkalmazni és hello alhálózatot elhagyó forgalomra ne befolyásolja az e szabály által.
  * "Prioritás", amelyben a forgalom áramlását ki lesz értékelve hello sorrend beállítása. hello alacsonyabb hello számú hello hello elsőbbséget. Ha egy szabály érvényes tooa adott adatforgalmat, nincsenek további szabályok feldolgozása. Így ha 1 prioritású szabály lehetővé teszi, hogy a forgalmat, és 2 prioritású szabály megtagadja a forgalmat, és mindkét szabályokat alkalmazni tootraffic akkor hello forgalom lesz engedélyezett tooflow (mivel szabály 1 kellett magasabb prioritású hatás tartott, és nincsenek további szabályok alkalmazása megtörtént).
  * "Access" jelzi, hogy a szabály által érintett forgalom-e letiltott ("Deny") vagy engedélyezett ("Engedélyezés").

    ```JSON
    "properties": {
    "securityRules": [
      {
        "name": "enable_dns_rule",
        "properties": {
          "description": "Enable Internal DNS",
          "protocol": "*",
          "sourcePortRange": "*",
          "destinationPortRange": "53",
          "sourceAddressPrefix": "VirtualNetwork",
          "destinationAddressPrefix": "10.0.2.4",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
    ```

3. Ez a szabály lehetővé teszi, hogy RDP forgalom tooflow hello internet toohello RDP-portjára hello bármely kiszolgálón az alhálózati kötött. 

    ```JSON
    {
      "name": "enable_rdp_rule",
      "properties": {
        "description": "Allow RDP",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "3389",
        "sourceAddressPrefix": "*",
        "destinationAddressPrefix": "*",
        "access": "Allow",
        "priority": 110,
        "direction": "Inbound"
      }
    },
    ```

4. Ez a szabály lehetővé teszi, hogy a bejövő internet forgalom toohit hello webkiszolgáló. Ez a szabály nem változtatja meg a hello útválasztási viselkedés. hello szabály csak a IIS01 toopass szánt-forgalmát engedélyezi. Így ha hello internetes forgalom volt-e a célként, ez a szabály akkor engedélyezi-e, és további szabályok feldolgozásának leállítása hello webkiszolgáló. (Hello szabályban prioritással 140 összes egyéb bejövő forgalmat le van tiltva). Ha csak a HTTP-forgalom éppen feldolgozás, ez a szabály lehet további korlátozott tooonly engedélyezi a 80-as Port cél.

    ```JSON
    {
      "name": "enable_web_rule",
      "properties": {
        "description": "Enable Internet too[variables('VM01Name')]",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "80",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "10.0.1.5",
        "access": "Allow",
        "priority": 120,
        "direction": "Inbound"
        }
      },
    ```

5. Ez a szabály lehetővé teszi, hogy forgalom toopass hello IIS01 kiszolgálóról toohello AppVM01 kiszolgáló, egy újabb szabály blokkolja a más előtér tooBackend forgalmat. Ez a szabály, ha hello port ismert, hogy hozzá kell adni tooimprove. Például ha hello IIS-kiszolgálón van elérte-e a AppVM01 csak SQL Server, hello Célporttartomány módosítani kell a "*" (minden) too1433 (hello SQL-port) így egy kisebb bejövő támadási felületét AppVM01 kell hello webalkalmazás legalább egyszer utaló jeleket.

    ```JSON
    {
      "name": "enable_app_rule",
      "properties": {
        "description": "Enable [variables('VM01Name')] too[variables('VM02Name')]",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "10.0.1.5",
        "destinationAddressPrefix": "10.0.2.5",
        "access": "Allow",
        "priority": 130,
        "direction": "Inbound"
      }
    },
     ```

6. Ez a szabály megtagadja a kiszolgálókról hello internet tooany hello hálózati forgalom. A 110-es és 120 prioritással hello szabályokat hello hatás tooallow csak bejövő internetes forgalom toohello tűzfal és a kiszolgálók RDP portok és blokkol minden más. Ez a szabály szerepel a "biztonságos" szabály tooblock összes váratlan adatfolyamok.

    ```JSON
    {
      "name": "deny_internet_rule",
      "properties": {
        "description": "Isolate hello [variables('VNetName')] VNet from hello Internet",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "VirtualNetwork",
        "access": "Deny",
        "priority": 140,
        "direction": "Inbound"
      }
    },
     ```

7. hello végső szabály megtagadja a forgalmat a hello Frontend alhálózathoz toohello Backend alhálózathoz. Mivel ez a szabály egy bejövő forgalomra vonatkozó csak szabály, akkor (érkező hello háttér toohello előtér) egy fordított forgalom nem engedélyezett.

    ```JSON
    {
      "name": "deny_frontend_rule",
      "properties": {
        "description": "Isolate hello [variables('Subnet1Name')] subnet from hello [variables('Subnet2Name')] subnet",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "[variables('Subnet1Prefix')]",
        "destinationAddressPrefix": "[variables('Subnet2Prefix')]",
        "access": "Deny",
        "priority": 150,
        "direction": "Inbound"
      }
    }
    ```

## <a name="traffic-scenarios"></a>Forgalom forgatókönyvek
#### <a name="allowed-internet-tooweb-server"></a>(*Engedélyezett*) tooweb internetkiszolgáló
1. Az internetes felhasználó egy HTTP-lap kér hello IIS01 NIC társított hálózati adapter hello hello nyilvános IP-címe
2. Nyilvános IP-cím hello forgalom toohello VNet IIS01 felé továbbítja (hello webkiszolgáló)
3. Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:
  1. NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály
  2. NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály
  3. NSG 3. szabály (Internet tooIIS01) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása
4. Forgalom találatok száma a belső kiszolgáló IP-címének hello webes IIS01 (10.0.1.5)
5. IIS01 a webes forgalom figyeli, ezt a kérelmet kap, és elindítja a hello kérés feldolgozása
6. IIS01 hello SQL Server a AppVM01 adatokat kéri
7. Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van
8. hello Backend alhálózathoz bejövő szabály feldolgozása kezdődik:
  1. NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály
  2. NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály
  3. NSG 3. szabály (Internet tooFirewall) nem vonatkozik, helyezze át a toonext szabály
  4. NSG 4. szabály (IIS01 tooAppVM01) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása
9. AppVM01 hello SQL-lekérdezést kap, és válaszolhat
10. Nincsenek kimenő szabályok a hello Backend alhálózathoz, mert engedélyezve van-e a hello válasz
11. Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:
  1. Nincs alkalmazó tooInbound forgalom hello háttér alhálózati toohello Frontend alhálózatból, így ha hello NSG-szabályok egyike NSG szabály
  2. hello alapértelmezett rendszerszabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így hello forgalom engedélyezve van.
12. hello IIS server hello SQL-válasz fogadása és hello HTTP-válasz befejeződik, és elküldi a toohello kérelmező
13. Mivel nincsenek kimenő szabályok hello Frontend alhálózaton, hello választ kap, és a hello internetes felhasználó megkapja a kért weblap hello.

#### <a name="allowed-rdp-tooiis-server"></a>(*Engedélyezett*) RDP tooIIS kiszolgáló
1. Egy kiszolgálói rendszergazda interneten kér egy RDP-munkamenet tooIIS01 a nyilvános IP-címe hello hello hello IIS01 hálózati adapter (a nyilvános IP-cím található hello portál vagy a PowerShell használatával) társított hálózati adapter
2. Nyilvános IP-cím hello forgalom toohello VNet IIS01 felé továbbítja (hello webkiszolgáló)
3. Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:
  1. NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály
  2. NSG szabály 2 (RDP) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása
4. A nem kimenő szabályokat alapértelmezett szabályokat alkalmazni, és a forgalom engedélyezve van
5. RDP-munkamenetbe engedélyezve van
6. Hello felhasználónevet és jelszót kér IIS01

>[!NOTE]
>tooRDP tooany háttérkiszolgálók ebben a példában hello IIS-kiszolgálón használt egy "ugrást mező." Első RDP toohello IIS-kiszolgálót és majd hello IIS Server RDP toohello háttér-kiszolgálóról.
>
>

#### <a name="allowed-web-server-dns-look-up-on-dns-server"></a>(*Engedélyezett*) a DNS-kiszolgáló Web server a DNS szolgáltatásban
1. Webalkalmazás-kiszolgáló, IIS01, kell egy adatcsatorna www.data.gov, azonban kell tooresolve hello cím.
2. hello tartozó fürthálózat-konfiguráció hello hálózatok listája DNS01 (a hello Backend alhálózathoz 10.0.2.4) hello elsődleges DNS-kiszolgálóként, IIS01 küldi hello DNS kérelem tooDNS01
3. Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van
4. Backend alhálózathoz bejövő szabály feldolgozása kezdődik:
  * NSG szabály 1 (DNS) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása
5. DNS-kiszolgáló hello kérést kap.
6. DNS-kiszolgáló nem rendelkezik gyorsítótárazott hello címmel és egy legfelső szintű DNS-kiszolgáló kéri a hello internet
7. Nincs kimenő szabályok a Backend alhálózathoz forgalom engedélyezve van
8. Internetes DNS-kiszolgáló válaszol, mivel ehhez a munkamenethez belső kezdeményezték, hello válasz engedélyezett
9. DNS-kiszolgáló gyorsítótárazza hello választ, és toohello kezdeti kérés hátsó tooIIS01 válaszol
10. Nincs kimenő szabályok a Backend alhálózathoz forgalom engedélyezve van
11. Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:
  1. Nincs alkalmazó tooInbound forgalom hello háttér alhálózati toohello Frontend alhálózatból, így ha hello NSG-szabályok egyike NSG szabály
  2. hello alapértelmezett rendszerszabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így hello forgalom engedélyezve van
12. IIS01 hello választ kap DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(*Engedélyezett*) server-hozzáférés fájl AppVM01
1. IIS01 kér AppVM01 fájlba
2. Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van
3. hello Backend alhálózathoz bejövő szabály feldolgozása kezdődik:
  1. NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály
  2. NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály
  3. NSG 3. szabály (Internet tooIIS01) nem vonatkozik, helyezze át a toonext szabály
  4. NSG 4. szabály (IIS01 tooAppVM01) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása
4. AppVM01 hello kérelmet kap, és válaszol, a fájl (feltéve, hogy van engedélyezve)
5. Nincsenek kimenő szabályok a hello Backend alhálózathoz, mert engedélyezve van-e a hello válasz
6. Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:
  1. Nincs alkalmazó tooInbound forgalom hello háttér alhálózati toohello Frontend alhálózatból, így ha hello NSG-szabályok egyike NSG szabály
  2. hello alapértelmezett rendszerszabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így hello forgalom engedélyezve van.
7. hello IIS kiszolgáló kap hello fájl

#### <a name="denied-rdp-toobackend"></a>(*Megtagadva*) RDP toobackend
1. Az internetes felhasználó megpróbál tooRDP tooserver AppVM01
2. Mivel nincs a kiszolgálók hálózati társított nyilvános IP-címek, ez az adatforgalom hello VNet soha ne adjon meg, és így elérni hello kiszolgálót
3. Azonban valamilyen okból engedélyezve volt a nyilvános IP-címnek, 2 (RDP) NSG-szabály lehetővé teszi az ilyen típusú adatforgalom

>[!NOTE]
>tooRDP tooany háttérkiszolgálók ebben a példában hello IIS-kiszolgálón használt egy "ugrást mező." Első RDP toohello IIS-kiszolgálót és majd hello IIS Server RDP toohello háttér-kiszolgálóról.
>
>

#### <a name="denied-web-toobackend-server"></a>(*Megtagadva*) toobackend webkiszolgáló
1. Az internetes felhasználó megpróbál tooaccess AppVM01 fájlba
2. Mivel nincs a kiszolgálók hálózati társított nyilvános IP-címek, ez az adatforgalom hello VNet soha ne adjon meg, és így elérni hello kiszolgálót
3. Valamilyen okból engedélyezve volt a nyilvános IP-címnek, NSG szabály 5 (Internet tooVNet) blokkolná-e a forgalom

#### <a name="denied-web-dns-look-up-on-dns-server"></a>(*Megtagadva*) DNS-kiszolgálón a webes DNS szolgáltatásban
1. Az internetes felhasználó megpróbál egy belső DNS-rekordot a DNS01 mentése toolook
2. Mivel nincs a kiszolgálók hálózati társított nyilvános IP-címek, ez az adatforgalom hello VNet soha ne adjon meg, és így elérni hello kiszolgálót
3. Valamilyen okból engedélyezve volt a nyilvános IP-címnek, NSG szabály 5 (Internet tooVNet) blokkolná-e a forgalom (Megjegyzés: hello internet és 1. szabály csak toohello vonatkozik, hogy szabály 1 (DNS) szeretné alkalmazni, mert hello kérelmek forráscím hello forrásaként virtuális helyi hálózat)

#### <a name="denied-sql-access-on-hello-web-server"></a>(*Megtagadva*) SQL-hozzáférés hello webkiszolgálón való üzemeltetésekor
1. Az internetes felhasználó SQL adatokat kér IIS01
2. Mivel nincs a kiszolgálók hálózati társított nyilvános IP-címek, ez az adatforgalom hello VNet soha ne adjon meg, és így elérni hello kiszolgálót
3. Ha egy nyilvános IP-cím valamilyen okból engedélyezve lett, hello Frontend alhálózathoz kezdődik bejövő szabály feldolgozása:
  1. NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály
  2. NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály
  3. NSG 3. szabály (Internet tooIIS01) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása
4. Forgalom találatok belső IP-címe hello IIS01 (10.0.1.5)
5. IIS01 1433-as portot, ezért nincs válasz toohello kérelmet nem figyeli.

## <a name="conclusion"></a>Összegzés
Ez a példa egy viszonylag egyszerű és egyszerű mód a hello háttér-alhálózathoz, a bejövő forgalom elkülönítésére.

További példákat és a hálózati biztonsági határokat egy áttekintést talál [Itt][HOME].

## <a name="references"></a>Referencia
### <a name="azure-resource-manager-template"></a>Azure Resource Manager-sablon
Ez a példa egy előre meghatározott Azure Resource Manager-sablon használja a Microsoft által kezelt GitHub-tárházban, és nyissa meg a toohello közösségi. Ez a sablon rögtön kívül a github webhelyen, telepíthetők, vagy letöltött és módosított toofit igényeinek. 

hello fő sablon megtalálható a hello fájlt "azuredeploy.json." Ez a sablon küldheti el PowerShell vagy a parancssori felületen keresztül (fájllal hello társított "azuredeploy.parameters.json") toodeploy ezt a sablont. Hello legegyszerűbb módja toouse hello "Központi telepítés tooAzure" gombra hello README.md oldalán a github webhelyen található.

Ebben a példában a Githubról, majd hello Azure-portálon épülő toodeploy hello sablon kövesse az alábbi lépéseket:

1. Egy böngészőből keresse meg a toohello [sablon][Template]
2. Kattintson hello "Központi telepítés tooAzure" (vagy hello "Visualize" gomb toosee sablon grafikus ábrázolása)
3. Hello Storage-fiók, felhasználónév és jelszó hello (paraméterek) panelen adja meg, majd kattintson a **OK**
5. Hozzon létre egy erőforráscsoportot a központi telepítés (egy meglévőt is használhat, de egy újat a legjobb eredmények elérése érdekében javasolt)
6. Szükség esetén módosítsa a virtuális hálózat hello előfizetésben és helyen beállításait.
7. Kattintson a **tekintse át a jogi feltételeket**, olvassa el hello feltételeit, és kattintson **beszerzési** tooagree.
8. Kattintson a **létrehozása** toobegin hello telepítési a sablonból.
9. Miután hello központi telepítés sikeresen befejeződött, keresse meg a központi telepítés toosee hello erőforrások konfigurált belül létrehozott erőforráscsoport toohello.

>[!NOTE]
>Ez a sablon lehetővé teszi az RDP toohello IIS01 kiszolgáló csak (Keresés hello IIS01 nyilvános IP-Címek a portál hello). tooRDP tooany háttérkiszolgálók ebben a példában hello IIS-kiszolgálón használt egy "ugrást mező." Első RDP toohello IIS-kiszolgálót és majd hello IIS Server RDP toohello háttér-kiszolgálóról.
>
>

tooremove a telepítés, a delete hello erőforráscsoportot és az összes gyermek-erőforrás is törlődik.

#### <a name="sample-application-scripts"></a>Alkalmazás mintaparancsfájlok
Hello sablon sikeres futtatása után állíthat be hello webkiszolgáló és az alkalmazáskiszolgáló rendelkező egy egyszerű webes alkalmazás tooallow e DMZ konfiguráció tesztelése. Ezzel és más DMZ példák mintaalkalmazás tooinstall, egy adtak meg, a következő hivatkozás hello: [minta alkalmazás-parancsfájl][SampleApp]

## <a name="next-steps"></a>Következő lépések

* Ez a példa központi telepítése
* Build hello mintaalkalmazás
* A DMZ keresztül különböző forgalom tesztelése

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-arm/example1design.png "Az NSG bejövő DMZ"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[Template]: https://github.com/Azure/azure-quickstart-templates/tree/master/301-dmz-nsg
[SampleApp]: ./virtual-networks-sample-app.md