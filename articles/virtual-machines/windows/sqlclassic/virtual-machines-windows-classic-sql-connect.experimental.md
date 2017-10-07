---
title: "SQL Server virtuális gép (klasszikus) Azure aaaConnect tooa |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconnect tooSQL Server rendszert futtató Azure virtuális gépen. Ez a témakör hello klasszikus üzembe helyezési modellt használ. hello forgatókönyvek különböznek attól függően, hello hálózati konfigurációs és hello ügyfél hello helyét."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-service-management
ms.assetid: 416948af-454f-4cfe-8fd2-7cf971cbd3e9
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/31/2017
ms.author: jroth
experimental_id: d51f3cc6-753b-4e
ms.openlocfilehash: 9577e4bdad79435e34a64fc669ec4848649fc945
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-sql-server-virtual-machine-on-azure-classic-deployment"></a>Csatlakozzon az SQL Server virtuális gépet az Azure (klasszikus üzembe helyezési) tooa
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-connect.md)
> * [Klasszikus](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a>Áttekintés
Ez a témakör ismerteti, hogyan tooconnect tooyour SQL Server példányt, egy Azure virtuális gépen futó. Bemutatja, néhány [általános kapcsolati forgatókönyvek](#connection-scenarios) , majd [egy Azure virtuális gép az SQL Server-kapcsolat beállításának lépései részletes](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Ha az erőforrás-kezelő virtuális gépeket használ, tekintse meg [csatlakozzon az SQL Server virtuális gépet az Azure Resource Manager használatával tooa](../sql/virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>Kapcsolat-forgatókönyvek
ügyfél hello módja attól függően, hogy hello helyét, valamint az ügyfél hello hello machine/hálózati konfigurációja eltér a virtuális gépen futó kiszolgáló tooSQL csatlakozik. Ezek a forgatókönyvek a következők:

* [TooSQL kiszolgáló csatlakozzon a hello ugyanaz a felhőalapú szolgáltatás](#connect-to-sql-server-in-the-same-cloud-service)
* [Csatlakozás tooSQL kiszolgálón keresztül hello internet](#connect-to-sql-server-over-the-internet)
* [Csatlakozzon a hello Server tooSQL azonos virtuális hálózatban](#connect-to-sql-server-in-the-same-virtual-network)

> [!NOTE]
> Ezek a módszerek bármelyikével csatlakozás előtt hajtsa végre az hello [Ez a cikk az tooconfigure összekapcsolhatóság szükséges lépések](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).
> 
> 

### <a name="connect-toosql-server-in-hello-same-cloud-service"></a>TooSQL kiszolgáló csatlakozzon a hello ugyanaz a felhőalapú szolgáltatás
Több virtuális gépek hozhatók létre hello ugyanaz a felhőalapú szolgáltatás. toounderstand a virtuális gépek forgatókönyv, lásd: [hogyan tooconnect virtuális gépek virtuális hálózati vagy a felhőalapú szolgáltatással](../classic/connect-vms.md#connect-vms-in-a-standalone-cloud-service). Ebben a forgatókönyvben egy virtuális gépen ügyfél megkísérel tooconnect tooSQL kiszolgáló fut egy másik virtuális gépén hello ugyanaz a felhőalapú szolgáltatás.

Ebben az esetben az hello VM használatával kapcsolódhatnak **neve** (is megjelennek az helyeként **számítógépnév** vagy **állomásnév** hello portálon). Ez az hello virtuális gép létrehozásakor megadott hello név. Például, ha az SQL virtuális gép elnevezett **mysqlvm**, használhatja ugyanazt a felhőalapú szolgáltatás hello ügyfél virtuális gép a következő kapcsolati karakterlánc tooconnect hello:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-toosql-server-over-hello-internet"></a>Kiszolgáló tooSQL hello interneten keresztüli kapcsolódás
Ha azt szeretné, hogy tooconnect tooyour SQL Server adatbázis motorja internethez hello, létre kell hoznia egy virtuális gép végpontjának bejövő TCP-kommunikációhoz. Az Azure-alapú konfigurációs lépés irányítja a bejövő TCP port forgalom tooa TCP-portot, amely elérhető toohello virtuális gépet.

tooconnect keresztül hello internet, hello VM DNS nevét és hello VM végpont portszámot (a cikk későbbi részében konfigurált) kell használnia. toofind hello DNS-nevét, keresse meg a toohello Azure portál, majd válassza **virtuális gépek (klasszikus)**. Ezután válassza ki a virtuális gép. Hello **DNS-név** megjelenik-e a hello **áttekintése** szakasz.

Vegyük példaként nevű klasszikus virtuális gép **mysqlvm** a DNS-név **mysqlvm7777.cloudapp.net** és egy virtuális gép végpontja **57500**. Ha megfelelően konfigurált kapcsolattal, a következő kapcsolati karakterlánc hello lehet használt tooaccess hello virtuális gép bárhonnan a hello internet:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Bár ez a kapcsolati karakterlánc kapcsolódást engedélyezi az ügyfelek keresztül hello internet, ez nem feltétlenül jelenti azt, hogy bárki kapcsolódhatnak-e az SQL Server tooyour. Külső ügyfelek toohello helyes felhasználónévvel és jelszóval rendelkezik. A fokozott biztonság érdekében ne használjon hello jól ismert 1433-as port hello nyilvános végpontot. És ha lehetséges, fontolja meg egy ACL ügyfeleken a végpont toorestrict forgalom csak toohello engedélyezi. Az ACL-ekkel végpontokon útmutatásért lásd: [kezelése hello ACL a végpont](../classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).

> [!NOTE]
> Ez a módszer toocommunicate használatakor egy SQL Server hello Azure datacenter kimenő adatok teljes-e a tulajdonos toonormal [a kimenő adatátviteli díjszabás](https://azure.microsoft.com/pricing/details/data-transfers/).
> 
> 

### <a name="connect-toosql-server-in-hello-same-virtual-network"></a>Csatlakozzon a hello Server tooSQL azonos virtuális hálózatban
[Virtuális hálózati](../../../virtual-network/virtual-networks-overview.md) további olyan forgatókönyveket tesz lehetővé. Hello azonos virtuális hálózatban, még akkor is, ha ezen virtuális gépek különböző felhőszolgáltatások szerepel a virtuális gépek is elérheti. És egy [telephelyek közötti VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), létrehozhat egy hibrid architektúra, amely a virtuális gépek a helyszíni hálózatokkal és gépek.

Virtuális hálózatok az Azure virtuális gépek tooa tartomány is engedélyezheti, toojoin. Csatlakozás tooa hello csak úgy toouse az SQL Server Windows-hitelesítés tartománya. hello más kapcsolat esetekben szükséges SQL-hitelesítés a felhasználónevek és jelszavak.

Ha a környezet tooconfigure és a Windows-hitelesítést, nem szükséges tooconfigure hello nyilvános végpontot vagy hello SQL-hitelesítést és bejelentkezések. Ebben a forgatókönyvben a hello kapcsolati karakterlánc hello SQL Server virtuális gép nevét megadva tooyour SQL Server-példány is elérheti. a következő példa hello feltételezi, hogy a Windows-hitelesítés konfigurálása és hello felhasználó nyert hozzáférést toohello SQL Server-példányt.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Egy Azure virtuális gép az SQL Server-kapcsolat beállításának lépései
hello lépések bemutatják, hogyan tooconnect toohello SQL Server-példány felett hello internet SQL Server Management Studio (SSMS) használatával. Azonban hello ugyanazokat a lépéseket alkalmazni toomaking az SQL Server virtuális gép elérhető, az alkalmazások, a helyszínen fut, és az Azure-ban.

Csatlakozzon az SQL Server toohello példányát egy másik virtuális gépről, vagy internetes hello, hajtsa végre a következő hello a következő feladatokat:

* [Hello virtuális gép TCP-végpont létrehozása](#create-a-tcp-endpoint-for-the-virtual-machine)
* [Nyissa meg a TCP-portok hello a Windows tűzfal](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
* [SQL Server toolisten konfigurálja a TCP protokoll hello](#configure-sql-server-to-listen-on-the-tcp-protocol)
* [SQL Server vegyes üzemmódú hitelesítés konfigurálása](#configure-sql-server-for-mixed-mode-authentication)
* [SQL Server-hitelesítési bejelentkezési létrehozása](#create-sql-server-authentication-logins)
* [Határozza meg a virtuális gép hello hello DNS-neve](#determine-the-dns-name-of-the-virtual-machine)
* [Adatbázis-kezelő toohello Csatlakozás másik számítógépről](#connect-to-the-database-engine-from-another-computer)

hello kapcsolati útvonal a következő diagram hello által összegzése:

![Csatlakozás tooa SQL Server virtuális gép](../../../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[!INCLUDE [Connect tooSQL Server in a VM Classic TCP Endpoint](../../../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[!INCLUDE [Connect tooSQL Server in a VM](../../../../includes/virtual-machines-sql-server-connection-steps.md)]

[!INCLUDE [Connect tooSQL Server in a VM Classic Steps](../../../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Következő lépések
Ha toouse AlwaysOn rendelkezésre állási csoportokat magas rendelkezésre állású és vész-helyreállítási is tervez, érdemes megfontolni egy figyelő megvalósítását. Adatbázis-ügyfelek csatlakoznak a toohello figyelő, hanem közvetlenül tooone hello SQL Server-példánya. hello figyelő útvonalak ügyfelek toohello elsődleges replika hello rendelkezésre állási csoportban. További információkért lásd: [egy ILB figyelőt az AlwaysOn rendelkezésre állási csoportok konfigurálása az Azure-](../classic/ps-sql-int-listener.md).

Ajánlott biztonsági eljárások az Azure virtuális gépen futó SQL Server összes hello fontos tooreview. További információkért lásd [az SQL Server Azure-beli virtuális gépeken történő futtatásának biztonsági szempontjait](../sql/virtual-machines-windows-sql-security.md).

[Fedezze fel hello képzési terv](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) az SQL Server Azure virtuális gépeken. 

Egyéb témakörök kapcsolódó toorunning SQL Server Azure virtuális gépeken, a következő témakörben: [SQL Server Azure virtuális gépeken](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

