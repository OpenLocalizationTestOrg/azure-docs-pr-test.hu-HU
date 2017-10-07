---
title: "egy külső figyelőt a Always On rendelkezésre állási csoportok aaaConfigure |} Microsoft Docs"
description: "Az oktatóanyag bemutatja, hogyan nyújt az Azure által kívülről elérhető használatával hozzon létre egy mindig a rendelkezésre állási csoport figyelőjének hello nyilvános virtuális IP-címe hello társított felhőalapú szolgáltatás."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a2453032-94ab-4775-b976-c74d24716728
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: mikeray
ms.openlocfilehash: f8e2110bcc25d9cb7653675cb4ae5c8d717b6902
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-external-listener-for-always-on-availability-groups-in-azure"></a>Egy külső figyelőt a Always On rendelkezésre állási csoportok konfigurálása az Azure-ban
> [!div class="op_single_selector"]
> * [Belső figyelő](../classic/ps-sql-int-listener.md)
> * [Külső figyelő](../classic/ps-sql-ext-listener.md)
> 
> 

Ez a témakör bemutatja, hogyan tooconfigure egy figyelőt a következő egy Always On rendelkezésre állási csoportnak kívülről elérhető hello internet. Ennek köszönhetően lehetséges hello felhőalapú szolgáltatás társításával **nyilvános virtuális IP-cím (VIP)** hello figyelő címmel.

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

A rendelkezésre állási csoport tartalmazhatnak replikákat, csak, a helyszíni Azure csak, vagy a helyszíni és az Azure span hibrid konfigurációk. Az Azure replikák elhelyezkedhetnek hello belül azonos régióban vagy több virtuális hálózatokról (Vnetekről) segítségével különféle régiókban. az alábbi hello lépések azt feltételezik, hogy már [konfigurált rendelkezésre állási csoport](../classic/portal-sql-alwayson-availability-groups.md) , de nincs beállítva a figyelő.

## <a name="guidelines-and-limitations-for-external-listeners"></a>Tudnivalók és korlátozások külső figyelők
Vegye figyelembe a következő irányelveket kapcsolatos hello rendelkezésre állási csoport figyelőjének az Azure-ban telepítésekor hello cloud service összeköttetés VIP címmel hello:

* hello rendelkezésre állási csoport figyelőjének Windows Server 2008 R2, Windows Server 2012 és Windows Server 2012 R2 esetén támogatott.
* egy másik felhőalapú szolgáltatást, mint a rendelkezésre állási csoport virtuális gépeket tartalmazó hello hello ügyfélalkalmazás kell lennie. Az Azure nem támogatja közvetlen kiszolgálói válasz ügyfél és kiszolgáló a hello ugyanaz a felhőalapú szolgáltatás.
* Alapértelmezés szerint a cikkben hello lépésekből megtudhatja, hogyan tooconfigure egy figyelő toouse hello felhőalapú szolgáltatás virtuális IP-cím (VIP) címét. Azonban lehetséges tooreserve, és hozzon létre több virtuális IP-címek a felhőalapú szolgáltatáshoz. Ez lehetővé teszi a cikk toocreate több figyelők, amelyek mindegyike különböző virtuális IP-címhez társított toouse hello lépéseit. Hogyan toocreate több virtuális IP-címek, meg információt [több virtuális IP-címek egy felhőalapú szolgáltatás](../../../load-balancer/load-balancer-multivip.md).
* Egy figyelő hibrid környezet létrehozásakor, hello a helyszíni hálózat rendelkeznie kell kapcsolattal toohello hozzáadása toohello a nyilvános Internet--webhelyek közötti VPN a hello Azure-beli virtuális hálózat. A hello Azure alhálózatban, hello rendelkezésre állási csoport figyelőjének érhető csak a megfelelő felhőszolgáltatás hello hello nyilvános IP-cím alapján.
* Nem támogatott egy külső-figyelője ugyanazt a felhőalapú szolgáltatás is esetében egy belső figyelővel hello toocreate hello belső Load Balancer (ILB).

## <a name="determine-hello-accessibility-of-hello-listener"></a>Határozza meg a hello kisegítő hello-figyelő
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Ez a cikk foglalkozik használó figyelő létrehozásával **külső terheléselosztás**. Ha egy figyelő, amely tooyour titkos virtuális hálózat, lásd: Ez a cikk beállításának lépéseit hello verzióját egy [ILB rendelkező figyelő](../classic/ps-sql-int-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Hozzon létre virtuális gép elosztott terhelésű végpontok közvetlen kiszolgálói visszatérési
Külső terheléselosztás hello virtuális hello nyilvános virtuális IP-címet használ hello felhőalapú szolgáltatás, amely a virtuális gépet futtat. Így nem kell toocreate vagy hello terheléselosztó ebben az esetben konfigurálása.

Az egyes virtuális gépek Azure replikájának üzemeltető, létre kell hoznia egy elosztott terhelésű végpont. Ha több régióba replikákat, minden egyes replikának régió nem lehet azonos a felhőalapú szolgáltatás a hello a hello ugyanazt a virtuális hálózatot. Több Azure-régiók kiterjedő replikák rendelkezésre állási csoport létrehozása több Vnetek beállítását igényli. Virtuális hálózat közötti kapcsolat konfigurálásával kapcsolatos további információkért lásd: [konfigurálása VNet tooVNet kapcsolat](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

1. Hello Azure-portálon lépjen a virtuális gép egy replika és a nézet hello részletei üzemeltetési tooeach.
2. Kattintson a hello **végpontok** hello virtuális gépek mindegyikének fülre.
3. Győződjön meg arról, hogy hello **neve** és **nyilvános Port** hello figyelő végpont kívánt toouse már nem használja. Hello az alábbi példában a hello neve: "MyEndpoint" hello port pedig "1433".
4. A helyi ügyfélen, töltse le és telepítse [hello legújabb PowerShell modul](https://azure.microsoft.com/downloads/).
5. Indítsa el **Azure PowerShell**. Egy új PowerShell-munkamenetet kell megnyitni hello Azure felügyeleti modulok terhelését.
6. Futtatás **Get-AzurePublishSettingsFile**. Ez a parancsmag tooa böngésző toodownload közzétételi beállítások fájl tooa helyi könyvtár irányítja. Kérheti a bejelentkezési hitelesítő adatok Azure-előfizetése.
7. Futtassa a hello **Import-AzurePublishSettingsFile** hello hello elérési parancs közzététele beállításfájl letöltött:
   
        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>
   
    Miután hello közzététele beállításfájl importálása, kezelheti az Azure-előfizetéshez hello PowerShell-munkamenetben.
    
1. Másolja az alábbi PowerShell-parancsprogramot hello egy szövegszerkesztőbe, és állítsa be a hello változók értékeinek toosuit a környezetben (alapértelmezett vannak-e megadva az egyes paraméterek). Vegye figyelembe, hogy a rendelkezésre állási csoport Azure-régiók is, ha futtassa a hello parancsfájl egyszer minden datacenter hello felhőalapú szolgáltatás, és azt az adatközpontot lévő csomópontok.
   
        # Define variables
        $ServiceName = "<MyCloudService>" # hello name of hello cloud service that contains hello availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in hello same cloud service, separated by commas
   
        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

2. Már megadta hello változók, másolása hello parancsfájl hello szövegszerkesztőben az Azure PowerShell-munkamenet toorun be azt. Ha még mindig hello kérdés >>, írja be a írja be újra toomake meg arról, hogy hello parancsfájl futásának indításakor.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Győződjön meg arról, hogy KB2854082 telepítve van-e, ha szükséges
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-hello-firewall-ports-in-availability-group-nodes"></a>Nyissa meg a rendelkezésre állási csoport csomópontok hello tűzfalportok
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-hello-availability-group-listener"></a>Hello rendelkezésre állási csoport figyelőjének létrehozása

Hello rendelkezésre állási csoport figyelőjének létrehozása két lépésben. Először hello ügyfél hozzáférési pont fürterőforrás létrehozása, és konfigurálja a függőségek. Ezután konfigurálja PowerShell hello fürt erőforrásokat.

### <a name="create-hello-client-access-point-and-configure-hello-cluster-dependencies"></a>Ügyfél-hozzáférési pont hello létrehozása és konfigurálása hello fürt függőségek
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-hello-cluster-resources-in-powershell"></a>A PowerShell hello fürterőforrások konfigurálása
1. A külső terheléselosztást, be kell szereznie hello nyilvános virtuális IP-cím hello felhőalapú szolgáltatás, amely tartalmazza a replikát. Jelentkezzen be hello Azure-portálon. Keresse meg a toohello felhőalapú szolgáltatás, amely tartalmazza a rendelkezésre állási csoport virtuális gép. Nyissa meg hello **irányítópult** nézet.
2. Vegye figyelembe a hello cím mezőben látható **nyilvános virtuális IP-cím (VIP)**. A megoldás Vnetek is, ha minden felhőalapú szolgáltatás, amely egy replikát tartalmazó ismételje meg ezt a lépést.
3. Hello virtuális gépek egyik másolja az alábbi PowerShell-parancsprogramot hello egy szövegszerkesztőbe, és hello változók korábban feljegyzett toohello értékeinek beállításához.
   
        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
        $IPResourceName = "<IPResourceName>" # hello IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service
   
        Import-Module FailoverClusters
   
        # If you are using Windows Server 2012 or higher, use hello Get-Cluster Resource command. If you are using Windows Server 2008 R2, use hello cluster res command. Both commands are commented out. Choose hello one applicable tooyour environment and remove hello # at hello beginning of hello line tooconvert hello comment tooan executable line of code.
   
        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255
4. Egyszer, hello változók van beállítva, nyisson meg egy rendszergazda jogú Windows PowerShell-ablakban, majd hello parancsfájl másolása hello szövegszerkesztőben és illessze be az Azure PowerShell-munkamenet toorun. Ha még mindig hello kérdés >>, írja be a írja be újra toomake meg arról, hogy hello parancsfájl futásának indításakor.
5. Ismételje meg ezt az összes virtuális Géphez. Ez a parancsfájl hello IP-cím erőforrás hello felhőszolgáltatás hello IP-címmel konfigurálja, és egyéb paramétereinek például hello mintavételi portot. Amikor hello IP-cím erőforrás online állapotba kerül, majd válaszolhassanak a hello mintavételi portot az ebben az oktatóanyagban korábban létrehozott hello elosztott terhelésű végpont lekérdezési toohello.

## <a name="bring-hello-listener-online"></a>Kapcsolja a hálózatra hello figyelő
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Követő műveletet elemek
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-hello-availability-group-listener-within-hello-same-vnet"></a>Teszt hello rendelkezésre állási csoport figyelőjét (belül hello azonos virtuális hálózaton)
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-hello-availability-group-listener-over-hello-internet"></a>Teszt hello rendelkezésre állási csoport figyelőjét (keresztül hello internet)
A sorrend tooaccess hello figyelő külső hello virtuális hálózatról, kell használnia terheléselosztás külső/nyilvános (a jelen témakörben ismertetett) helyett ILB, amely csak hello belül elérhető ugyanazt a virtuális hálózatot. Hello kapcsolati karakterláncot hello felhőalapú szolgáltatás nevét kell megadni. Például, ha egy hello nevű felhőszolgáltatás *mycloudservice*, hello sqlcmd utasítás a következőképpen nézne ki:

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

Hello előző példában eltérően SQL-hitelesítést kell használható, mert hello hívó hello keresztül nem használja a windows-hitelesítést internetes. További információkért lásd: [Always On rendelkezésre állási csoportnak Azure virtuális gépen: ügyfél-csatlakozási forgatókönyvek](http://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx). Amikor az SQL-hitelesítést használ, győződjön meg arról, hogy hozzon létre hello mindkét replikák azonos bejelentkezés. Rendelkezésre állási csoportokkal bejelentkezések hibaelhárítással kapcsolatos további információkért lásd: [hogyan toomap bejelentkezést, vagy használjon tárolt SQL adatbázis-felhasználó tooconnect tooother replikák és hozzárendelését az egyes tooavailability adatbázisok](http://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx).

Ha külön alhálózatokon vannak hello mindig a replikákat, ügyfeleknek meg kell adniuk **MultisubnetFailover = True** hello kapcsolati karakterláncban. Az eredmény párhuzamos kapcsolódási kísérletek tooreplicas hello különböző alhálózatokon. Vegye figyelembe, hogy ez az eset tartalmazza-e a központi telepítés kereszt-régió Always On rendelkezésre állási csoportnak.

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]

