---
title: "aaaAzure DMZ példa – Build az NSG-ket egy egyszerű DMZ |} Microsoft Docs"
description: "A hálózati biztonsági csoportokkal (NSG) DMZ összeállítása"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: f8622b1d-c07d-4ea6-b41c-4ae98d998fff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 32a40a8dc7539c4c7293988e6c36e5e32ef11045
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-classic-powershell"></a>1 – például egy egyszerű DMZ NSG-k használata a klasszikus PowerShell összeállítása
[Visszatérési toohello biztonsági határ bevált gyakorlatok lap][HOME]

> [!div class="op_single_selector"]
> * [Resource Manager-sablon](virtual-networks-dmz-nsg.md)
> * [Klasszikus - PowerShell](virtual-networks-dmz-nsg-asm.md)
> 
>

Ebben a példában egy egyszerű DMZ négy Windows-kiszolgálók és hálózati biztonsági csoportokat hoz létre. Ez a példa bemutatja az egyes hello vonatkozó PowerShell parancsok tooprovide bemutatják az egyes lépések. Szerepel továbbá egy forgalom forgatókönyv szakasz tooprovide egy részletesebb lépésenkénti hogyan forgalom halad keresztül hello szolgálhat a hello DMZ. Végezetül hello references szakaszában van hello teljes kód látható és utasítás toobuild a környezet tootest és a különböző forgatókönyvekben a kísérletet. 

![Az NSG bejövő DMZ][1]

## <a name="environment-description"></a>Környezet leírása
Ebben a példában egy előfizetés tartalmazza a következő erőforrások hello:

* A felhőalapú szolgáltatások két: "FrontEnd001" és "BackEnd001"
* A virtuális hálózat, a "CorpNetwork", a két alhálózat; "Előtér" és "Háttér"
* A hálózati biztonsági csoport, amely alkalmazott tooboth alhálózatok
* A Windows Server egy webalkalmazás-kiszolgáló ("IIS01") jelölő
* Két windows Server kiszolgálókon, amelyek megfelelnek az alkalmazás háttérkiszolgálók ("AppVM01", "AppVM02")
* A Windows server DNS-kiszolgáló ("DNS01") jelölő

Hello references szakaszában egy PowerShell-parancsfájlt, amely ebben a példában leírt hello környezetben a legtöbb buildek van. Épület hello virtuális gépek és virtuális hálózatok, bár hello a példaként megadott parancsfájlt, által végzett dokumentum nem ismerteti a jelen dokumentum részletesen. 

toobuild hello környezet;

1. Hello hálózati konfigurációs XML-fájl (nevét, helyét és IP címeket toomatch megadott hello forgatókönyv frissíti) hello references szakaszában szereplő mentése
2. Frissítés hello felhasználói változók hello parancsfájl toomatch hello környezet hello parancsfájlban toobe futtatni (előfizetések, service nevét stb.)
3. PowerShell hello parancsfájlok végrehajtását

>[!Note]
>hello régió hello PowerShell-parancsfájl esetében meg kell egyeznie a hello hálózati konfigurációs XML-fájlban szereplő hello régióban.
>
>

Miután hello parancsfájl futtatása sikeresen további választható lépéseket lehet tenni, hello references szakaszában két parancsfájlok tooset hello webkiszolgáló és egy egyszerű webes alkalmazás tooallow e DMZ konfiguráció tesztelése az alkalmazások kiszolgálói.

hello következő szakaszokban részletes leírása a hálózati biztonsági csoportok és működésének ehhez a példához keresztül hello PowerShell parancsfájl sorok érdekében.

## <a name="network-security-groups-nsg"></a>Hálózati biztonsági csoportok (NSG)
Az ebben a példában egy NSG-csoport összeállítása és hat szabályokkal majd betölteni. 

> [!TIP]
> Általánosságban véve kell először hozza létre az adott "Engedélyezés" szabályokat, és majd hello utolsó általánosabbá "Deny" szabályokat. hello prioritású határozzák meg, hogy mely szabályokat értékeli ki a rendszer először. Forgalom található tooapply tooa meghatározott szabály, ha nincsenek további szabályok kiértékelése. NSG-szabályok alkalmazhat vagy hello bejövő vagy kimenő irányban (szempontjából hello hello alhálózati).
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

Nincs olyan alapértelmezett kimenő szabály, amely lehetővé teszi, hogy a toohello kimenő forgalom internet. Ebben a példában a Microsoft most átengedi a kimenő forgalmat, és nem módosítja az egyetlen kimenő szabályok. toolock le mindkét irányú forgalmát, felhasználói definiált útválasztás szükség, és a"3" hello a felfedezte van [biztonsági határ bevált gyakorlatok lap][HOME].

Minden egyes szabály tárgyalt részletesebben az alábbiak szerint (**Megjegyzés**: valamely elemére a következő lista kezdődő dollárjelet hello (például: $NSGName) egy felhasználó által definiált változó hello parancsfájlból hello hivatkozás szakaszban a jelen dokumentum):

1. Először egy hálózati biztonsági csoportot kell kialakítani, toohold hello szabályok:

    ```PowerShell
    New-AzureNetworkSecurityGroup -Name $NSGName `
        -Location $DeploymentLocation `
        -Label "Security group for $VNetName subnets in $DeploymentLocation"
    ```

2. Ebben a példában a hello első szabály lehetővé teszi, hogy a DNS-forgalom hello backend alhálózathoz összes belső hálózatok toohello DNS-kiszolgáló között. hello szabály néhány fontos paraméterekkel rendelkezik:
   
   * "Típus" azt jelzi, hogy a forgalom iránya a szabály lép érvénybe. hello használata hello szempontjából hello alhálózati vagy a virtuális gép (attól függően, ahol az NSG kötve van). Így ha típus: "Bejövő" és a forgalom hello alhálózati írja be, hello szabályt alkalmazni és hello alhálózatot elhagyó forgalomra ne befolyásolja az e szabály által.
   * "Prioritás", amelyben a forgalom áramlását ki lesz értékelve hello sorrend beállítása. hello alacsonyabb hello számú hello hello elsőbbséget. Ha egy szabály érvényes tooa adott adatforgalmat, nincsenek további szabályok feldolgozása. Így ha 1 prioritású szabály lehetővé teszi, hogy a forgalmat, és 2 prioritású szabály megtagadja a forgalmat, és mindkét szabályokat alkalmazni tootraffic akkor hello forgalom lesz engedélyezett tooflow (mivel szabály 1 kellett magasabb prioritású hatás tartott, és nincsenek további szabályok alkalmazása megtörtént).
   * "Action" azt jelzi, hogy ha a szabály által érintett forgalom vagy tiltott.

    ```PowerShell    
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
        -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[4] `
        -DestinationPortRange '53' `
        -Protocol *
    ```

3. Ez a szabály lehetővé teszi, hogy RDP forgalom tooflow hello internet toohello RDP-portjára hello bármely kiszolgálón az alhálózati kötött. Ez a szabály címelőtagokat; különleges kétféle használ. "VIRTUAL_NETWORK" és "INTERNET". Ezek a címkék egy egyszerűen tooaddress címelőtagokat nagyobb kategóriáját.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable RDP too$VNetName VNet" `
         -Type Inbound -Priority 110 -Action Allow `
         -SourceAddressPrefix INTERNET -SourcePortRange '*' `
         -DestinationAddressPrefix VIRTUAL_NETWORK `
         -DestinationPortRange '3389' `
         -Protocol *
    ```

4. Ez a szabály lehetővé teszi, hogy a bejövő internet forgalom toohit hello webkiszolgáló. Ez a szabály nem változtatja meg a hello útválasztási viselkedés. hello szabály csak a IIS01 toopass szánt-forgalmát engedélyezi. Így ha hello internetes forgalom volt-e a célként, ez a szabály akkor engedélyezi-e, és további szabályok feldolgozásának leállítása hello webkiszolgáló. (Hello szabályban prioritással 140 összes egyéb bejövő forgalmat le van tiltva). Ha csak a HTTP-forgalom éppen feldolgozás, ez a szabály lehet további korlátozott tooonly engedélyezi a 80-as Port cél.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable Internet too$VMName[0]" `
         -Type Inbound -Priority 120 -Action Allow `
         -SourceAddressPrefix Internet -SourcePortRange '*' `
         -DestinationAddressPrefix $VMIP[0] `
         -DestinationPortRange '*' `
         -Protocol *
    ```

5. Ez a szabály lehetővé teszi, hogy forgalom toopass hello IIS01 kiszolgálóról toohello AppVM01 kiszolgáló, egy újabb szabály blokkolja a más előtér tooBackend forgalmat. Ez a szabály, ha hello port ismert, hogy hozzá kell adni tooimprove. Például ha hello IIS-kiszolgálón van elérte-e a AppVM01 csak SQL Server, hello Célporttartomány módosítani kell a "*" (minden) too1433 (hello SQL-port) így egy kisebb bejövő támadási felületét AppVM01 kell hello webalkalmazás legalább egyszer utaló jeleket.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] too$VMName[2]" `
        -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
    ```

6. Ez a szabály megtagadja a kiszolgálókról hello internet tooany hello hálózati forgalom. A 110-es és 120 prioritással hello szabályokat hello hatás tooallow csak bejövő internetes forgalom toohello tűzfal és a kiszolgálók RDP portok és blokkol minden más. Ez a szabály szerepel a "biztonságos" szabály tooblock összes váratlan adatfolyamok.
    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate hello $VNetName VNet from hello Internet" `
        -Type Inbound -Priority 140 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *
    ```
7. hello végső szabály megtagadja a forgalmat a hello Frontend alhálózathoz toohello Backend alhálózathoz. Mivel ez a szabály egy bejövő forgalomra vonatkozó csak szabály, akkor (érkező hello háttér toohello előtér) egy fordított forgalom nem engedélyezett.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate hello $FESubnet subnet from hello $BESubnet subnet" `
        -Type Inbound -Priority 150 -Action Deny `
        -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
        -DestinationAddressPrefix $BEPrefix `
        -DestinationPortRange '*' `
        -Protocol * 
    ```

## <a name="traffic-scenarios"></a>Forgalom forgatókönyvek
#### <a name="allowed-internet-tooweb-server"></a>(*Engedélyezett*) tooweb internetkiszolgáló
1. Az internetes felhasználó egy HTTP-lap kér FrontEnd001.CloudApp.Net (Internet Facing Felhőszolgáltatás)
2. Cloud service fázisok forgalom nyitott végpontok felé IIS01 80-as porton keresztül (hello webkiszolgáló)
3. Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:
   1. NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály
   2. NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály
   3. NSG 3. szabály (Internet tooIIS01) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása
4. Forgalom találatok száma a belső kiszolgáló IP-címének hello webes IIS01 (10.0.1.5)
5. IIS01 a webes forgalom figyeli, ezt a kérelmet kap, és elindítja a hello kérés feldolgozása
6. IIS01 hello SQL Server a AppVM01 adatokat kéri
7. Nincsenek kimenő szabályok a Frontend alhálózathoz, mert a forgalom engedélyezve van-e
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
13. Mivel nincsenek kimenő szabályok hello Frontend alhálózaton hello választ kap, és hello internet felhasználó kap a kért weblap hello.

#### <a name="allowed-rdp-toobackend"></a>(*Engedélyezett*) RDP toobackend
1. Server Admin interneten kérelmek RDP munkamenet tooAppVM01 BackEnd001.CloudApp.Net:xxxxx, ahol xxxxx az RDP tooAppVM01 véletlenszerűen hello portszámát a (hozzárendelt hello port található hello Azure-portálon vagy a PowerShell segítségével)
2. Backend alhálózathoz bejövő szabály feldolgozása kezdődik:
   1. NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály
   2. NSG szabály 2 (RDP) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása
3. A nem kimenő szabályokat alapértelmezett szabályokat alkalmazni, és a forgalom engedélyezve van
4. RDP-munkamenetbe engedélyezve van
5. Hello felhasználónevet és jelszót kér AppVM01

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

#### <a name="denied-web-toobackend-server"></a>(*Megtagadva*) toobackend webkiszolgáló
1. Az internetes felhasználó megpróbál tooaccess keresztül hello BackEnd001.CloudApp.Net szolgáltatás AppVM01 fájlba
2. Mivel nincsenek nyissa meg a fájlmegosztás nincsenek végpontok, a forgalom hello felhőalapú szolgáltatás nem lesz továbbítja, és így elérni hello kiszolgálót
3. Ha valamilyen okból hello végpontok nyitott, NSG szabály 5 (Internet tooVNet) blokkolná-e a forgalom

#### <a name="denied-web-dns-look-up-on-dns-server"></a>(*Megtagadva*) DNS-kiszolgálón a webes DNS szolgáltatásban
1. Az internetes felhasználó megpróbál toolook fel egy belső DNS-rekordot a DNS01 keresztül hello BackEnd001.CloudApp.Net szolgáltatás
2. Mivel nincsenek végpontok nyissa meg a DNS, a forgalom hello felhőalapú szolgáltatás nem lesz továbbítja, és így elérni hello kiszolgálót
3. Ha valamilyen okból hello végpontok nyitott, NSG szabály 5 (Internet tooVNet) blokkolná-e a forgalom (Megjegyzés: két okból nem kell alkalmazni, hogy a szabály 1 (DNS), első hello forráscím van hello internet, ez a szabály csak érvényes toohello hello mint virtuális helyi hálózat is forrás Ez a szabály szerepel egy olyan engedélyezési szabály, akkor soha nem letiltsa a forgalom)

#### <a name="denied-web-toosql-access-through-firewall"></a>(*Megtagadva*) webalkalmazás-tooSQL hozzáférés tűzfalon keresztül
1. Az internetes felhasználó SQL adatokat kér FrontEnd001.CloudApp.Net (Internet Facing Felhőszolgáltatás)
2. Mivel nincsenek végpontok nyissa meg az SQL, a forgalom hello felhőalapú szolgáltatás nem lesz továbbítja, és így elérni hello tűzfal
3. Ha valamilyen okból végpontok voltak nyitva, hello Frontend alhálózathoz megkezdi a bejövő szabály feldolgozása:
   1. NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály
   2. NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály
   3. NSG 3. szabály (Internet tooIIS01) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása
4. Forgalom találatok belső IP-címe hello IIS01 (10.0.1.5)
5. IIS01 1433-as portot, ezért nincs válasz toohello kérelmet nem figyeli.

## <a name="conclusion"></a>Összegzés
Ez a példa egy viszonylag egyszerű és egyszerű mód a hello háttér-alhálózathoz, a bejövő forgalom elkülönítésére.

További példákat és a hálózati biztonsági határokat egy áttekintést talál [Itt][HOME].

## <a name="references"></a>Referencia
### <a name="main-script-and-network-config"></a>Fő parancsfájlt és a hálózati konfiguráció
Hello teljes parancsfájl menthető egy olyan PowerShell-parancsfájlt. Hálózati konfiguráció hello "NetworkConf1.xml." fájlba mentése
Hello felhasználói változók módosításához szükséges és futtatási hello parancsfájlként.

#### <a name="full-script"></a>Teljes szkript
Ez a parancsfájl fog hello felhasználói változók; alapján

1. Csatlakozás Azure-előfizetés tooan
2. Create a storage account
3. Hozzon létre egy virtuális hálózat és két alhálózat hello hálózati konfigurációs fájlban meghatározottak szerint
4. Négy windows server virtuális gépek létrehozása
5. Adja meg, beleértve az NSG:
   * Az NSG létrehozása
   * Azt a szabályoknak feltöltése
   * Kötési hello NSG toohello megfelelő alhálózatokat

A PowerShell parancsfájl fusson helyben egy internethez csatlakoztatott PC vagy a kiszolgáló.

> [!IMPORTANT]
> Ezt a parancsfájlt, amikor előfordulhat figyelmeztetések vagy a PowerShellben pop más tájékoztató üzeneteit. Piros csak hibaüzenetek aggodalomra.
> 
>

```PowerShell
<# 
 .SYNOPSIS
  Example of Network Security Groups in an isolated network (Azure only, no hybrid connections)

 .DESCRIPTION
  This script will build out a sample DMZ setup containing:
   - A default storage account for VM disks
   - Two new cloud services
   - Two Subnets (FrontEnd and BackEnd subnets)
   - One server on hello FrontEnd Subnet
   - Three Servers on hello BackEnd Subnet
   - Network Security Groups tooallow/deny traffic patterns as declared

  Before running script, ensure hello network configuration file is created in
  hello directory referenced by $NetworkConfigFile variable (or update the
  variable tooreflect hello path and file name of hello config file being used).

 .Notes
  Security requirements are different for each use case and can be addressed in a
  myriad of ways. Please be sure that any sensitive data or applications are behind
  hello appropriate layer(s) of protection. This script serves as an example of some
  of hello techniques that can be used, but should not be used for all scenarios. You
  are responsible tooassess your security needs and hello appropriate protections
  needed, and then effectively implement those protections.

  FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
   IIS01      - 10.0.1.5

  BackEnd Service (BackEnd subnet 10.0.2.0/24)
   DNS01      - 10.0.2.4
   AppVM01    - 10.0.2.5
   AppVM02    - 10.0.2.6

#>

# Fixed Variables
    $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password toobe used for all VMs"
    $VMName = @()
    $ServiceName = @()
    $VMFamily = @()
    $img = @()
    $size = @()
    $SubnetName = @()
    $VMIP = @()

# User-Defined Global Variables
  # These should be changes tooreflect your subscription and services
  # Invalid options will fail in hello validation section

  # Subscription Access Details
    $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

  # VM Account, Location, and Storage Details
    $LocalAdmin = "theAdmin"
    $DeploymentLocation = "Central US"
    $StorageAccountName = "vmstore02"

  # Service Details
    $FrontEndService = "FrontEnd001"
    $BackEndService = "BackEnd001"

  # Network Details
    $VNetName = "CorpNetwork"
    $FESubnet = "FrontEnd"
    $FEPrefix = "10.0.1.0/24"
    $BESubnet = "BackEnd"
    $BEPrefix = "10.0.2.0/24"
    $NetworkConfigFile = "C:\Scripts\NetworkConf1.xml"

  # VM Base Disk Image Details
    $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

  # NSG Details
    $NSGName = "MyVNetSG"

# User-Defined VM Specific Configuration
    # Note: tooensure proper NSG Rule creation later in this script:
    #       - hello Web Server must be VM 0
    #       - hello AppVM1 Server must be VM 1
    #       - hello DNS server must be VM 3
    #
    #       Otherwise hello NSG rules in hello last section of this
    #       script will need toobe changed toomatch hello modified
    #       VM array numbers ($i) so hello NSG Rule IP addresses
    #       are aligned toohello associated VM IP addresses.

    # VM 0 - hello Web Server
      $VMName += "IIS01"
      $ServiceName += $FrontEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $FESubnet
      $VMIP += "10.0.1.5"

    # VM 1 - hello First Application Server
      $VMName += "AppVM01"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.5"

    # VM 2 - hello Second Application Server
      $VMName += "AppVM02"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.6"

    # VM 3 - hello DNS Server
      $VMName += "DNS01"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.4"

# ----------------------------- #
# No User-Defined Variables or  #
# Configuration past this point #
# ----------------------------- #    

  # Get your Azure accounts
    Add-AzureAccount
    Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
    Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

  # Create Storage Account
    If (Test-AzureName -Storage -Name $StorageAccountName) { 
        Write-Host "Fatal Error: This storage account name is already in use, please pick a different name." -ForegroundColor Red
        Return}
    Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
          New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}

  # Update Subscription Pointer tooNew Storage Account
    Write-Host "Updating Subscription Pointer tooNew Storage Account" -ForegroundColor Cyan 
    Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

# Validation
$FatalError = $false

If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
     Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
     $FatalError = $true}

If (Test-AzureName -Service -Name $FrontEndService) { 
    Write-Host "hello FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
    $FatalError = $true}
Else { Write-Host "hello FrontEndService service name is valid for use." -ForegroundColor Green}

If (Test-AzureName -Service -Name $BackEndService) { 
    Write-Host "hello BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
    $FatalError = $true}
Else { Write-Host "hello BackEndService service name is valid for use." -ForegroundColor Green}

If (-Not (Test-Path $NetworkConfigFile)) { 
    Write-Host 'hello network config file was not found, please update hello $NetworkConfigFile variable toopoint toohello network config xml file.' -ForegroundColor Yellow
    $FatalError = $true}
Else { Write-Host "hello network configuration file was found" -ForegroundColor Green
        If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
            Write-Host 'hello deployment location was not found in hello network config file, please check hello network config file tooensure hello $DeploymentLocation variable is correct and hello network config file matches.' -ForegroundColor Yellow
            $FatalError = $true}
        Else { Write-Host "hello deployment location was found in hello network config file." -ForegroundColor Green}}

If ($FatalError) {
    Write-Host "A fatal error has occurred, please see hello above messages for more information." -ForegroundColor Red
    Return}
Else { Write-Host "Validation passed, now building hello environment." -ForegroundColor Green}

# Create VNET
    Write-Host "Creating VNET" -ForegroundColor Cyan 
    Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

# Create Services
    Write-Host "Creating Services" -ForegroundColor Cyan
    New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
    New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop

# Build VMs
    $i=0
    $VMName | Foreach {
        Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
        New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
            Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
            Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
            Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
            Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
            Remove-AzureEndpoint -Name "PowerShell" | `
            New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
            # Note: A Remote Desktop endpoint is automatically created when each VM is created.
        $i++
    }
    # Add HTTP Endpoint for IIS01
    Get-AzureVM -ServiceName $ServiceName[0] -Name $VMName[0] | Add-AzureEndpoint -Name HTTP -Protocol tcp -LocalPort 80 -PublicPort 80 | Update-AzureVM

# Configure NSG
    Write-Host "Configuring hello Network Security Group (NSG)" -ForegroundColor Cyan

  # Build hello NSG
    Write-Host "Building hello NSG" -ForegroundColor Cyan
    New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

  # Add NSG Rules
    Write-Host "Writing rules into hello NSG" -ForegroundColor Cyan
    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[3] -DestinationPortRange '53' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP too$VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet too$($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
        -SourceAddressPrefix Internet -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[0]) too$($VMName[1])" -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[0] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[1] -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet from hello Internet" -Type Inbound -Priority 140 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $FESubnet subnet from hello $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
        -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
        -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
        -Protocol *

    # Assign hello NSG toohello Subnets
        Write-Host "Binding hello NSG tooboth subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

# Optional Post-script Manual Configuration
  # Install Test Web App (Run Post-Build Script on hello IIS Server)
  # Install Backend resource (Run Post-Build Script on hello AppVM01)
  Write-Host
  Write-Host "Build Complete!" -ForegroundColor Green
  Write-Host
  Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
  Write-Host " - Install Test Web App (Run Post-Build Script on hello IIS Server)" -ForegroundColor Gray
  Write-Host " - Install Backend resource (Run Post-Build Script on hello AppVM01)" -ForegroundColor Gray
  Write-Host
```

#### <a name="network-config-file"></a>Hálózati konfigurációs fájl
Az XML-fájl mentése frissített hellyel rendelkező, és hello hivatkozás toothis fájl toohello $NetworkConfigFile változó hozzáadása a hello parancsfájlt.

```XML
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
  <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="DNS01" IPAddress="10.0.2.4" />
        <DnsServer name="Level3" IPAddress="209.244.0.3" />
      </DnsServers>
    </Dns>
    <VirtualNetworkSites>
      <VirtualNetworkSite name="CorpNetwork" Location="Central US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="FrontEnd">
            <AddressPrefix>10.0.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="BackEnd">
            <AddressPrefix>10.0.2.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
        <DnsServersRef>
          <DnsServerRef name="DNS01" />
          <DnsServerRef name="Level3" />
        </DnsServersRef>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
  </VirtualNetworkConfiguration>
</NetworkConfiguration>
```

#### <a name="sample-application-scripts"></a>Alkalmazás mintaparancsfájlok
Ha a tooinstall mintaalkalmazás ezzel és más DMZ példák, egy adtak meg, a következő hivatkozás hello: [minta alkalmazás-parancsfájl][SampleApp]

## <a name="next-steps"></a>Következő lépések
* Frissítse és mentse az XML-fájl
* Hello PowerShell parancsfájl toobuild hello környezetet
* Hello mintaalkalmazás telepítése
* A DMZ keresztül különböző forgalom tesztelése

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Az NSG bejövő DMZ"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

