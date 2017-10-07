---
title: "az első Azure virtuális hálózat aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy Azure virtuális hálózatot (VNet), csatlakozás két virtuális gépek (VM) toohello virtuális hálózaton, és toohello virtuális gépek csatlakozni."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2016
ms.author: jdial
ms.openlocfilehash: 1981524cf706d5ebc83b1ff77735617550ff058a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-virtual-network"></a>Az első virtuális hálózat létrehozása

Megtudhatja, hogyan toocreate egy virtuális hálózatot (VNet), két alhálózattal, hozzon létre két virtuális gép (VM), valamint elérheti mindegyik virtuális gép tooone hello alhálózatokat, ahogy az alábbi képen hello:

![Virtuális hálózat diagramja](./media/virtual-network-get-started-vnet-subnet/vnet-diagram.png)

Egy Azure virtuális hálózatot (VNet) a saját hálózati hello felhőben megjelenítése. Vezérelheti az Azure-hálózat beállításait, valamint DHCP-címblokkokat, DNS-beállításokat, biztonsági házirendeket és útválasztásokat adhat meg. További információk a virtuális hálózat fogalmakat, olvassa el a hello toolearn [Virtual Network áttekintése](virtual-networks-overview.md) cikk. Hajtsa végre a következő lépéseket toocreate hello erőforrások hello képen látható hello:

1. [Virtuális hálózat létrehozása két alhálózattal](#create-vnet)
2. [Hozzon létre két virtuális gép, egy hálózati csatoló (NIC) rendelkező](#create-vms), és rendelje hozzá a hálózati biztonsági csoport (NSG) tooeach hálózati adapter
3. [Tooand csatlakoztatja a virtuális gépek hello](#connect-to-from-vms)
4. [Az összes erőforrás törlése](#delete-resources). Az egyes létrehozott ebben a gyakorlatban, amíg azok van kiépítve hello erőforrásokhoz függő díj terheli. toominimize hello költségek, hello a gyakorlatban befejezése után ne feledje toocomplete hello szükséges lépések Ez a szakasz toodelete hello erőforrásokat hoz létre.

Hogy alapszinten megértse, hogyan használhatja a VNet után épp hello ebben a cikkben ismertetett visszaállítási lépésekkel. További lépések találhatók, így kapcsolatos részletesebb toouse Vnetek mélyebb ismeretére.

## <a name="create-vnet"></a>Virtuális hálózat létrehozása két alhálózattal

toocreate egy virtuális hálózatot, két alhálózattal, teljes hello szükséges lépésekről. Különböző alhálózatokon jellemzően használt toocontrol hello folyamata az alhálózatok közötti forgalmat.

1. Jelentkezzen be toohello [Azure-portálon](<https://portal.azure.com>). Ha még nincs fiókja, regisztrálhat egy [egy hónapos ingyenes próbaverzióra](https://azure.microsoft.com/free). 
2. A hello **Kedvencek** ablaktáblán megjelenő hello portál **új**.
3. A hello **új** panelen kattintson a **hálózati**. A hello **hálózati** panelen kattintson a **virtuális hálózati**, ahogy az alábbi képen hello:

    ![Virtuális hálózat diagramja](./media/virtual-network-get-started-vnet-subnet/virtual-network.png)

4.  A hello **virtuális hálózati** panelen hagyja *erőforrás-kezelő* választotta hello üzembe helyezési modellt, majd kattintson **létrehozása**.
5.  A hello **hozzon létre virtuális hálózat panel** , amely akkor jelenik meg, írja be a következő értékek hello, majd kattintson a **létrehozása**:

    |**Beállítás**|**Érték**|**Részletek**|
    |---|---|---|
    |**Name (Név)**|*MyVNet*|hello nevének hello erőforráscsoporton belül egyedinek kell lennie.|
    |**Címtér**|*10.0.0.0/16*|Tetszés szerint bármilyen címteret megadhat a CIDR-jelölésrendszerben.|
    |**Alhálózat neve**|*Előtér*|hello alhálózati név hello virtuális hálózaton belül egyedinek kell lennie.|
    |**Alhálózati címtartomány**|*10.0.0.0/24*| megadott hello tartományon belül hello virtuális hálózathoz megadott hello címterület léteznie kell.|
    |**Előfizetés**|*[Az Ön előfizetése]*|Válasszon egy előfizetés toocreate hello virtuális hálózat található. A virtuális hálózat egyetlen előfizetésben jön létre.|
    |**Erőforráscsoport**|**Új létrehozása:***MyRG*|Hozzon létre egy erőforráscsoportot. az erőforráscsoport neve hello kiválasztott hello előfizetésen belül egyedinek kell lennie. További tudnivalók az erőforráscsoportokról, olvassa el a hello toolearn [erőforrás-kezelő](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups) a cikk áttekintése.|
    |**Hely**|*USA nyugati régiója*| Általában hello helye a legközelebb tooyour fizikai hely van kiválasztva.|

    virtuális hálózat vesz néhány másodperc toocreate hello. Ha létrehozták, akkor tekintse meg a hello Azure portál Irányítópultjára.

6. Hello hello Azure-portálon létrehozott virtuális hálózattal **Kedvencek** ablaktáblán kattintson a **összes erőforrás**. Kattintson a hello **MyVNet** hello a virtuális hálózati **összes erőforrás** panelen. Ha már kiválasztott hello előfizetés több erőforrást tartalmaz, megadhatja *MyVNet* a hello **Szűrés név alapján...** mezőbe tooeasily hozzáférés hello virtuális hálózat.
7. Hello **MyVNet** panel nyílik meg, és hello VNet, információkat jelenít meg, ahogy az alábbi képen hello:

    ![Virtuális hálózat diagramja](./media/virtual-network-get-started-vnet-subnet/myvnet.png)

8. Hello előző ábrán látható, kattintson a **alhálózatok** toodisplay hello virtuális hálózaton belül hello alhálózatok listája. hello csak olyan alhálózatot, amely létezik van **előtér-**, 5. lépésben létrehozott alhálózati hello.
9. Hello MyVNet - alhálózatok paneljén kattintson **+ alhálózati** toocreate hello alhálózat információkat, majd kattintson a következő **OK** toocreate hello alhálózati:

    |**Beállítás**|**Érték**|**Részletek**|
    |---|---|---|
    |**Name (Név)**|*Háttér*|hello nevének hello virtuális hálózaton belül egyedinek kell lennie.|
    |**Címtartomány**|*10.0.1.0/24*|megadott hello tartományon belül hello virtuális hálózathoz megadott hello címterület léteznie kell.|
    |**Hálózati biztonsági csoport** és **Útválasztási táblázat**|*Nincs* (alapértelmezett)|A hálózati biztonsági csoportok (NSG-k) leírása a cikk későbbi részében található. További információk a felhasználó által definiált útvonalak, olvassa el a hello toolearn [felhasználó által definiált útvonalak](virtual-networks-udr-overview.md) cikk.|

10. Új alhálózat hello toohello virtuális hálózat hozzáadása után bezárhatja a hello **MyVNet – alhálózatok** panelen, majd zárja be hello **összes erőforrás** panelen.

## <a name="create-vms"></a>Virtuális gépek létrehozása

Hello hálózatok és alhálózatok létrehozott hello virtuális gépeket hozhat létre. Ehhez a gyakorlathoz mindkét hello Windows Server operációs rendszert futtató virtuális gépek azonban futtathatják Azure, beleértve a több különböző Linux terjesztések átviteli által támogatott operációs rendszert.

### <a name="create-web-server-vm"></a>Hello webkiszolgáló virtuális gép létrehozása

toocreate hello webkiszolgáló virtuális gép, teljes hello a következő lépéseket:

1. Hello Azure portál Kedvencek ablaktábláján kattintson **új**, **számítási**, majd **Windows Server 2016 Datacenter**.
2. A hello **Windows Server 2016 Datacenter** panelen kattintson a **létrehozása**.
3. A hello **alapjai** panelen megjelenő, adja meg vagy válassza ki a következő értékek hello, majd kattintson **OK**:

    |**Beállítás**| **Érték**|**Részletek**|
    |---|---|---|
    |**Name (Név)**|*MyWebServer*|Ez a virtuális gép fog webkiszolgálóként szolgálni, amelyhez az internetes erőforrások csatlakoznak.|
    |**Virtuális merevlemez típusa**|*SSD*|
    |**Felhasználónév**|*A választása szerint*|
    |**Jelszó és Jelszó megerősítése**|*A választása szerint*|
    | **Előfizetés**|*<Your subscription>*|hello előfizetés kell ugyanahhoz az előfizetéshez hello 5. lépésben kiválasztott hello [hozzon létre egy virtuális hálózatot, két alhálózattal](#create-vnet) című szakaszát. hello csatlakoztat egy virtuális gép toomust virtuális hálózat szerepel hello azonos módon hello VM előfizetés.|
    |**Erőforráscsoport**|**Meglévő használata:** Válassza a *MyRG* elemet|Abban az esetben, ha ugyanabban az erőforráscsoportban hasonlóan hello VNet hello hello használunk erőforrások tooexist nincs hello ugyanabban az erőforráscsoportban.|
    |**Hely**|*USA nyugati régiója*|hello helynek kell lennie ugyanazon a helyen hello 5. lépésben megadott hello [hozzon létre egy virtuális hálózatot, két alhálózattal](#create-vnet) című szakaszát. Virtuális gépek és hello toomust csatlakozáshoz Vnetek szerepel hello azonos helyen.|

4. A hello **méret kiválasztása** panelen kattintson a *DS1_V2 szabványos*, kattintson a **kiválasztása**. Olvasási hello [Windows Virtuálisgép-méretek](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikkében Azure által támogatott összes Windows Virtuálisgép-méretek listáját.
5. A hello **beállítások** panelen adja meg vagy válassza ki a következő értékek hello, majd kattintson **OK**:

    |**Beállítás**|**Érték**|**Részletek**|
    |---|---|---|
    |**Tároló: Felügyelt lemezek használata**|*Igen*||
    |**Virtuális hálózat**| Válassza a *MyVNet* elemet|Kiválaszthatja, hogy minden virtuális hálózat már szerepel hello azonos módon hello hoz létre virtuális gép helyét. További információk a virtuális hálózatokat és alhálózatokat, olvassa el a hello toolearn [virtuális hálózati](virtual-networks-overview.md) cikk.|
    |**Alhálózat**|Válassza ki az *Előtér* elemet|Kiválaszthatja, hogy egyetlen alhálózatának sem, hogy létezik hello Vneten belül.|
    |**Nyilvános IP-cím**|Fogadja el az alapértelmezett hello|A nyilvános IP-cím lehetővé teszi, hogy Ön tooconnect toohello VM hello Internet a. További információ a nyilvános IP-címek, olvassa el a hello toolearn [IP-címek](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) cikk.|
    |**Hálózati biztonsági csoport (tűzfal)**|Fogadja el az alapértelmezett hello|Kattintson a hello **(új) MyWebServer-nsg** alapértelmezett NSG hello portálon létrehozott tooview annak beállításait. A hello **hálózati biztonsági csoport létrehozása** panelt megnyitó, figyelje meg, hogy van egy bejövő szabályt, amely bármely IP-forráscím TCP/3389-es (RDP)-forgalmát engedélyezi.|
    |**Minden egyéb érték**|Fogadja el hello alapértelmezett|További információk a fennmaradó beállításokat, olvassa el a hello hello toolearn [kapcsolatos virtuális gépek](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikk.|

    Hálózati biztonsági csoportokkal (NSG) lehetővé teszi a bejövő/kimenő szabályokat toocreate hello típusú hálózati forgalmat, amely a virtuális gép hello tooand áramolhasson. Alapértelmezés szerint minden bejövő forgalom toohello VM megtagadva. Az éles webkiszolgáló esetében további bejövő szabályokat adhat hozzá a TCP/80 (HTTP) és a TCP/443 (HTTPS) portok számára. Nincs a kimenő forgalomra vonatkozó szabály, mivel alapértelmezés szerint minden kimenő forgalom engedélyezett. Akkor is hozzáadása szabályok toocontrol forgalom a házirendek száma. Olvasási hello [hálózati biztonsági csoportok](virtual-networks-nsg.md) cikk toolearn további információk az NSG-k.

6.  A hello **összegzés** panelt, tekintse át a hello beállításokat, majd kattintson **OK** toocreate hello virtuális gép. Egy állapota csempe jelenik meg a portál irányítópultján hello hello VM hoz létre. Eltarthat néhány percig toocreate. Nincs szükség a toowait az toocomplete. Toohello a következő lépés a virtuális gép kerül létrehozásra hello közben folytathatja.

### <a name="create-database-server-vm"></a>Hello adatbázis-kiszolgáló virtuális gép létrehozása

toocreate hello adatbázis-kiszolgáló virtuális gépek teljes hello a következő lépéseket:

1.  Hello Kedvencek ablaktábláján kattintson **új**, **számítási**, majd **Windows Server 2016 Datacenter**.
2.  A hello **Windows Server 2016 Datacenter** panelen kattintson a **létrehozása**.
3.  A hello **alapvető beállítások panel**, adja meg vagy válassza ki a következő értékek hello, majd **OK**:

    |**Beállítás**|**Érték**|**Részletek**|
    |---|---|---|
    |**Name (Név)**|*MyDBServer*|Adatbázis-kiszolgálót, hogy hello webkiszolgáló csatlakozik, de Internet adott hello nem lehet csatlakozni a virtuális gép szolgál.|
    |**Virtuális merevlemez típusa**|*SSD*||
    |**Felhasználónév**|A választása szerint||
    |**Jelszó és Jelszó megerősítése**|A választása szerint||
    |**Előfizetés**|<Your subscription>|hello előfizetés kell ugyanahhoz az előfizetéshez hello 5. lépésben kiválasztott hello [hozzon létre egy virtuális hálózatot, két alhálózattal](#create-vnet) című szakaszát.|
    |**Erőforráscsoport**|**Meglévő használata:** Válassza a *MyRG* elemet|Abban az esetben, ha ugyanabban az erőforráscsoportban hasonlóan hello VNet hello hello használunk erőforrások tooexist nincs hello ugyanabban az erőforráscsoportban.|
    |**Hely**|*USA nyugati régiója*|hello helynek kell lennie ugyanazon a helyen hello 5. lépésben megadott hello [hozzon létre egy virtuális hálózatot, két alhálózattal](#create-vnet) című szakaszát.|

4.  A hello **méret kiválasztása** panelen kattintson a *DS1_V2 szabványos*, kattintson a **kiválasztása**.
5.  A hello **beállítások** panelen adja meg vagy válassza ki a következő értékek hello, majd kattintson **OK**:

    |**Beállítás**|**Érték**|**Részletek**|
    |----|----|---|
    |**Tároló: Felügyelt lemezek használata**|*Igen*||
    |**Virtuális hálózat**|Válassza a *MyVNet* elemet|Kiválaszthatja, hogy minden virtuális hálózat már szerepel hello azonos módon hello hoz létre virtuális gép helyét.|
    |**Alhálózat**|Válassza ki *háttér-* hello kattintva **alhálózati** jelölését, jelölje be **háttér-** a hello **alhálózat kiválasztása** panel|Kiválaszthatja, hogy egyetlen alhálózatának sem, hogy létezik hello Vneten belül.|
    |**Nyilvános IP-cím**|Nincs – hello alapértelmezett címet, majd **nincs** a hello **nyilvános IP-cím kiválasztása** panel|Egy nyilvános IP-cím nem lehet csak csatlakoztatni toohello virtuális gép egy másik virtuális gép csatlakoztatott toohello ugyanazt a virtuális hálózatot. Közvetlenül a hello Internet tooit nem lehet csatlakoztatni.|
    |**Hálózati biztonsági csoport (tűzfal)**|Fogadja el az alapértelmezett hello| Például hello alapértelmezett NSG-t is létre hello MyWebServer VM az NSG tartozik hello azonos alapértelmezett bejövő szabály. Hozzáadhat egy további bejövő szabályt a TCP/1433-as (MS SQL) porthoz egy adatbázis-kiszolgáló esetén. Nincs a kimenő forgalomra vonatkozó szabály, mivel alapértelmezés szerint minden kimenő forgalom engedélyezett. Akkor is hozzáadása szabályok toocontrol forgalom a házirendek száma.|
    |**Minden egyéb érték**|Fogadja el hello alapértelmezett||

6.  A hello **összegzés** panelt, tekintse át a hello beállításokat, majd kattintson **OK** toocreate hello virtuális gép. Egy állapota csempe jelenik meg a portál irányítópultján hello hello VM hoz létre. Eltarthat néhány percig toocreate. Nincs szükség a toowait az toocomplete. Toohello a következő lépés a virtuális gép kerül létrehozásra hello közben folytathatja.

## <a name="review"></a>Erőforrások áttekintése

Ha létrehozott egy virtuális hálózat és két virtuális gépeket, hello Azure-portálon létrehozott több további erőforrásokat meg hello MyRG erőforráscsoportban. Tekintse át a hello MyRG erőforráscsoport hello tartalmát hello lépések végrehajtásával:

1. A hello **Kedvencek** ablaktáblán kattintson a **további szolgáltatások**.
2. A hello **további szolgáltatások** ablak, írja be *erőforráscsoportok* hello be, amely rendelkezik hello word *szűrő* azt. Kattintson a **erőforráscsoportok** amikor látható hello szűrt listája.
3. A hello **erőforráscsoportok** ablaktáblán kattintson a hello *MyRG* erőforráscsoport. Ha sok meglévő erőforráscsoportok az előfizetéshez, akkor írja be *MyRG* hello mezőben hello szöveget tartalmazó *Szűrés név alapján...* tooquickly hozzáférés hello MyRG erőforráscsoportot.
4.  A hello **MyRG** panelen láthatja, hogy hello erőforráscsoport 12 erőforrásai, ahogy az alábbi képen hello:

    ![Erőforráscsoport tartalma](./media/virtual-network-get-started-vnet-subnet/resource-group-contents.png)

További információk a virtuális gépek, a lemezek és a storage-fiókok, olvassa el a hello toolearn [virtuális gép](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [lemez](../virtual-machines/windows/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-network%2ftoc.json), és [tárfiók](../storage/common/storage-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) áttekintése cikkeket. Hello két alapértelmezett NSG-ket hello portál létrehozza a tekintheti meg. Megtekintheti az adott hello portálon létrehozott két hálózati illesztő (NIC) erőforrásokat is. A hálózati adapter lehetővé teszi, hogy a virtuális gép tooconnect tooother erőforrások hello virtuális hálózaton keresztül. Olvasási hello [NIC](virtual-network-network-interface.md) cikk toolearn további információk a hálózati adapterek. hello portálon is létrejön, egy nyilvános IP-cím erőforrás. A Nyilvános IP-címek az egyik beállítás a nyilvános IP-cím típusú erőforrásokhoz. További információ a nyilvános IP-címek, olvassa el a hello toolearn [IP-címek](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) cikk.

## <a name="connect-to-from-vms"></a>Csatlakozás toohello virtuális gépek

A virtuális hálózat és két virtuális gépeket létrehozni csatlakoztathatja toohello virtuális gépek a következő részekben hello hello lépések végrehajtásával:

### <a name="connect-from-internet"></a>Toohello web server rendszerű virtuális Géphez kapcsolódó hello Internet

tooconnect toohello-webkiszolgáló virtuális gép hello interneten keresztül, teljes hello a következő lépéseket:

1. Hello portálon, nyissa meg hello MyRG erőforráscsoport hello elvégzésével szükséges lépések hello [tekintse át az erőforrásokat](#review) című szakaszát.
2. A hello **MyRG** panelen kattintson a hello **MyWebServer** virtuális gép.
3. A hello **MyWebServer** panelen kattintson a **Connect**, ahogy az alábbi képen hello:

    ![Csatlakozás tooweb server rendszerű virtuális Géphez](./media/virtual-network-get-started-vnet-subnet/webserver.png)

4. A böngésző toodownload hello engedélyezése *MyWebServer.rdp* fájlt, majd nyissa meg azt.
5. Ha megjelenik egy párbeszédpanel bezárásához tájékoztatása meg, hogy hello hello távoli kapcsolat közzétevője nem ellenőrizhető, kattintson a **Connect**.
6. Ha a hitelesítő adatok megadása, győződjön meg arról, hello felhasználónevet és jelszót hello 3. lépésében megadott bejelentkezési [létrehozás hello webkiszolgáló virtuális gép](#create-web-server-vm) című szakaszát. Ha hello **Windows biztonsági** meg nem tartalmazza a hello megfelelő hitelesítő adatokkal, szükséges lehet a tooclick **több lehetőséget**, majd **használjon más fiókot**, így meg lehet Adjon meg hello helyes felhasználónevet és jelszót). Kattintson a **OK** tooconnect toohello virtuális gép.
7. Ha megjelenik egy **távoli asztali kapcsolat** mezőben, amely tájékoztatja, hogy hello hello távoli számítógép identitása nem ellenőrizhető, kattintson a **Igen**.
8. Az hello Internet csatlakoztatott toohello MyWebServer VM is. Hagyja bejelölve hello távoli asztali kapcsolat megnyitása toocomplete hello lépéseket hello a következő szakaszban.

távoli kapcsolat hello toohello nyilvános IP-cím toohello nyilvános IP cím erőforrás hello portal hello 5. lépésében létrehozott [hozzon létre egy virtuális hálózatot, két alhálózattal](#create-vnet) című szakaszát. hello kapcsolat engedélyezett, mert hello létre hello alapértelmezett szabály **MyWebServer-nsg** TCP/3389-es (RDP) megengedett NSG bejövő toohello VM bármilyen forrás IP-címről. Ha tooconnect toohello VM bármely más porton keresztül, hello nem sikerül, ha további bejövő szabályok toohello NSG lehetővé tevő hello további portokat nem.

>[!NOTE]
>Ha további bejövő szabályok toohello NSG-t ad hozzá, győződjön meg arról, hogy hello ugyanazokat a portokat nyissa meg a Windows tűzfal hello, vagy hello kapcsolat meghiúsul.
>

### <a name="connect-to-internet"></a>Csatlakozás toohello Internet hello webkiszolgáló virtuális gép

tooconnect kimenő toohello Internet hello webkiszolgáló virtuális gép, teljes hello a következő lépéseket:

1. Ha még nem rendelkezik a távoli kapcsolat toohello MyWebServerVM megnyitásához, ellenőrizze a távoli kapcsolat toohello VM hello lépések végrehajtásával a hello [Connect toohello-webkiszolgáló VM hello Internet](#connect-from-internet) című szakaszát.
2. Hello Windows asztaláról nyissa meg az Internet Explorer. A hello **beállítása az Internet Explorer 11** párbeszédpanelen kattintson **ajánlott beállítások használata nem**, majd kattintson a **OK**. Ajánlott beállítások az üzemi kiszolgáló ajánlott tooaccept hello.
3. Hello Internet Explorer címsorába írja be a [bing.com](http:www.bing.com). Az Internet Explorer párbeszédpanel megjelenésekor kattintson **Hozzáadás**, majd **Hozzáadás** a hello **megbízható helyek** párbeszédpanel megnyitásához, és kattintson **Bezárás**. Ismételje meg ezt a műveletet az összes többi Internet Explorer-párbeszédpanelen is.
4. A hello Bing keresése oldal, adja meg *whatsmyipaddress*, majd kattintson a hello Nagyítót ábrázoló gombra. Bing hello nyilvános IP cím toohello nyilvános IP-cím erőforrás hello portál által létrehozott hello virtuális gép létrehozása után adja vissza. Ha megvizsgálja hello hello beállításainak **MyWebServer-ip** erőforrás, lásd: hello toohello nyilvános IP-cím erőforrás, azonos IP-cím, ahogy az alábbi hello kép. IP-cím tooyour hello VM eltér azonban.

    ![Csatlakozás tooweb server rendszerű virtuális Géphez](./media/virtual-network-get-started-vnet-subnet/webserver-pip.png)

5.  Hagyja bejelölve hello távoli asztali kapcsolat megnyitása toocomplete hello lépéseket hello a következő szakaszban.

Mivel alapértelmezés szerint engedélyezve van a virtuális gép hello minden kimenő kapcsolat képes tooconnect toohello Internet a virtuális gép hello áll. Korlátozhatja a kimenő kapcsolat hozzáadásával hozzáadása szabályok toohello alkalmazott NSG toohello NIC, toohello alhálózati hello hálózati adapter csatlakozik-e, vagy mindkettőt.

Virtuális gép hello kerül, ha hello hello portál használatával (felszabadított) állapot Leállítva, hello nyilvános IP-címet módosíthatja. Győződjön meg arról, hogy hello nyilvános IP-cím soha ne módosítsa veszi van szüksége, ha hello dinamikus kiosztási módszerrel (amely hello alapértelmezett), hanem hello IP-cím használható hello statikus kiosztási módszerrel. hello arról toolearn hello foglalási módszerek közötti különbségeket olvasási [IP-cím, típusok és foglalási módszerek](virtual-network-ip-addresses-overview-arm.md) cikk.

### <a name="webserver-to-dbserver"></a>Csatlakozás toohello adatbázis-kiszolgáló virtuális gép hello webkiszolgáló virtuális gép

tooconnect toohello adatbázis-kiszolgálójának VM hello webkiszolgáló virtuális gép, teljes hello a következő lépéseket:

1. Ha még nem rendelkezik a távoli kapcsolat toohello MyWebServer VM megnyitásához, ellenőrizze a távoli kapcsolat toohello VM hello lépések végrehajtásával a hello [Connect toohello-webkiszolgáló VM hello Internet](#connect-from-internet) című szakaszát.
2. Hello bal alsó sarkában hello Windows asztali hello Start gombra, majd kezdje el begépelni *távoli asztal*. Ha hello Start menü listája **távoli asztali kapcsolat**, kattintson rá.
3. A hello **távoli asztali kapcsolat** párbeszédpanelen adja meg a *Saját_adatbázis_kiszolgáló* hello számítógép nevét, majd kattintson **Connect**.
4. Hello felhasználói név és hello 3. lépésében megadott jelszavak [létrehozás hello adatbázis-kiszolgáló virtuális gép](#create-database-server-vm) szakasz ebben a cikkben, majd kattintson a **OK**.
5. Ha megjelenik egy párbeszédpanel bezárásához tájékoztatása meg, hogy hello hello távoli számítógép identitása nem ellenőrizhető, kattintson a **Igen**.
6. Hagyja hello távoli asztali kapcsolat megnyitása toocomplete hello lépések hello a következő szakaszban tooboth kiszolgálók.

Biztosan tudni toomake hello kapcsolat toohello adatbázis-kiszolgálójának VM hello webkiszolgáló virtuális gép a következő okok miatt hello:

- TCP/3389-es bejövő kapcsolatok engedélyezve vannak a forrás IP-címhez NSG hello 5. lépésben létrehozott hello alapértelmezett [létrehozás hello adatbázis-kiszolgáló virtuális gép](#create-database-server-vm) című szakaszát.
- Hello kapcsolat hello webkiszolgáló virtuális gép, amely csatlakoztatott toohello indította el adatbázis-kiszolgálóként hello VM ugyanazt a virtuális hálózatot. tooconnect tooa virtuális Gépet, amely nem rendelkezik a nyilvános IP-cím tooit, csatlakoznia kell egy másik virtuális gép csatlakoztatott toohello az azonos virtuális hálózaton, akkor is, ha a virtuális gép hello csatlakoztatott tooa másik alhálózat.
- Annak ellenére, hogy hello virtuális gépek csatlakoztatott toodifferent alhálózatok, az Azure alapértelmezett útvonalakat, amelyek lehetővé teszik az alhálózatok közötti kapcsolatot hoz létre. Hozzon létre egy saját azonban felülbírálhatja hello alapértelmezett útvonalak. Olvasási hello [felhasználó által definiált útvonalak](virtual-networks-udr-overview.md) cikk toolearn további információk az Azure-ban útválasztást.

Ha tooinitiate egy távoli kapcsolat toohello adatbázis-kiszolgálójának VM hello Internet, mint a hello [Connect toohello-webkiszolgáló VM hello Internet](#connect-from-internet) szakasz ebben a cikkben az adott hello látja **Connect** beállítás szürkén jelenik meg. Csatlakozás nem érhető el, mert nincs nyilvános IP-cím toohello VM, így az hello Internet a bejövő kapcsolatok tooit nem lehetséges.

### <a name="connect-toohello-internet-from-hello-database-server-vm"></a>Toohello Internet csatlakoztatja a hello adatbázis-kiszolgáló virtuális gép

Kapcsolódás kimenő toohello Internet-kiszolgálóról hello VM hello lépések elvégzésével:

1. Ha még nem rendelkezik egy Saját_adatbázis_kiszolgáló VM hello MyWebServer VM nyissa meg a távoli kapcsolat toohello, teljes hello hello szükséges lépések [Connect toohello adatbázis-kiszolgáló virtuális gép hello webkiszolgáló virtuális gép](#webserver-to-dbserver) című szakaszát.
2. A hello Saját_adatbázis_kiszolgáló VM hello a Windows asztalon, nyissa meg az Internet Explorert, és toohello párbeszédpanelek válaszoljon, mint a 2. és 3. hello [toohello Internet csatlakozás hello webkiszolgáló virtuális gép](#connect-to-internet) című szakaszát.
3. Hello címsorába írja be a [bing.com](http:www.bing.com).
4. Kattintson a **Hozzáadás** hello Internet Explorer megjelenő párbeszédpanelen, majd a **Hozzáadás**, majd **Bezárás** a hello **megbízható** helyek párbeszédpanel. Hajtsa végre ezeket a lépéseket az összes többi megjelenő párbeszédpanelen is.
5. A hello Bing keresése oldal, adja meg *whatsmyipaddress*, majd kattintson a hello Nagyítót ábrázoló gombra. Bing hello nyilvános IP cím aktuálisan hozzárendelt toohello VM hello Azure-infrastruktúra ad vissza. 6. Zárja be a hello távoli asztali toohello Saját_adatbázis_kiszolgáló VM a hello MyWebServer virtuális gép, majd zárja be a hello távoli kapcsolat toohello MyWebServer virtuális gép.

hello kimenő kapcsolat toohello Internet használata engedélyezett, mert minden kimenő forgalom alapértelmezés szerint engedélyezve van annak ellenére, hogy a nyilvános IP-cím erőforrás nincs hozzárendelve toohello Saját_adatbázis_kiszolgáló virtuális gép. Minden virtuális gép alapértelmezés szerint képesek tooconnect kimenő toohello internetes, vagy egy nyilvános IP cím hozzárendelt erőforrás toohello VM anélkül. Még nem tud tooconnect toohello nyilvános IP-cím a hello Internet azonban például képes toofor hello MyWebServer virtuális Gépet, amely rendelkezik egy nyilvános IP-cím erőforrás cím volt.

## <a name="delete-resources"></a>Az összes erőforrás törlése

toodelete összes erőforrás létrehozása ebben a cikkben teljes hello a következő lépéseket:

1. tooview hello MyRG erőforráscsoport létrehozásánál Ez a cikk teljes lépések az 1-3 hello [tekintse át az erőforrásokat](#review) című szakaszát. Ebben az esetben tekintse át a hello erőforrások hello erőforráscsoportban. Ha létrehozott hello MyRG erőforráscsoportot, egy előző lépést, lásd: hello 12 erőforrások a 4. lépésben hello képen látható.
2. Hello MyRG paneljén kattintson hello **törlése** gombra.
3. hello portal meg tootype hello nevét, amelyet az toodelete hello erőforrás csoport tooconfirm azt. Ha látja erőforrások eltérő hello 4. lépésben megjelenített hello erőforrások [tekintse át az erőforrásokat](#review) szakasz ebben a cikkben kattintson **Mégse**. Ha csak ez a cikk részeként létrehozott hello 12 erőforrásokat, írja be a *MyRG* hello erőforráscsoport-nevet, majd kattintson **törlése**. Erőforráscsoport törlésével törli az összes erőforrás hello erőforráscsoporton belül, ezért mindig meg arról, hogy tooconfirm erőforráscsoport hello tartalmának törlése előtt. hello portal hello erőforráscsoporton belül található összes erőforrást törli, majd törli a hello erőforráscsoport magát. Ez a folyamat több percig is eltarthat.

## <a name="next-steps"></a>Következő lépések

Ebben a példában egy VNetet és két virtuális gépet hozott létre. A virtuális gépek létrehozása során megadott néhány egyéni beállítást, valamint elfogadott több alapértelmezett beállítást is. Azt javasoljuk, hogy olvassa el a következő cikkeket, éles Vnetek és virtuális gépeket, hogy tudomásul veszi, hogy az összes rendelkezésre álló beállítások tooensure telepítése előtt hello:

- [Virtuális hálózatok](virtual-networks-overview.md)
- [Nyilvános IP-címek](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses)
- [Hálózati illesztők](virtual-network-network-interface.md)
- [Hálózati biztonsági csoportok](virtual-networks-nsg.md)
- [Virtuális gépek](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
