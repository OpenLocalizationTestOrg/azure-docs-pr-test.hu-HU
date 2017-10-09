---
title: "aaaConfigure hello Always On rendelkezésre állási csoport egy Azure virtuális gépen a PowerShell használatával |} Microsoft Docs"
description: "Ez az oktatóanyag hello klasszikus üzembe helyezési modellel létrehozott erőforrások használja. Az Azure PowerShell toocreate Always On rendelkezésre állási csoport használja."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a4e2f175-fe56-4218-86c7-a43fb916cc64
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: d4a27e203b2ff299adebec2b010c03422459b3c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-always-on-availability-group-on-an-azure-vm-with-powershell"></a>Hello Always On rendelkezésre állási csoport konfigurálásához egy Azure virtuális gépen a PowerShell használatával
> [!div class="op_single_selector"]
> * [Klasszikus: felhasználói felület](../classic/portal-sql-alwayson-availability-groups.md)
> * [Klasszikus: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
<br/>

Mielőtt elkezdené, vegye figyelembe, hogy most hajthatja végre ezt a feladatot az Azure resource manager modellt. Azt javasoljuk, hogy az Azure resource manager modellt új központi telepítéséhez. Lásd: [SQL Server Always On rendelkezésre állási csoportok az Azure virtuális gépeken](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

> [!IMPORTANT]
> Azt javasoljuk, hogy az új telepítések esetén hello Resource Manager modellt használja. Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja.

Az Azure virtuális gépek (VM) segítségével adatbázis rendszergazdák toolower hello költség magas rendelkezésre állású SQL Server rendszert. Az oktatóanyag bemutatja, hogyan tooimplement rendelkezésre állási csoporthoz az SQL Server Always On-végpontok belül környezetet az Azure használatával. Hello az oktatóanyag végén hello az SQL Server Always On megoldás az Azure-ban a következő elemek hello áll:

* Virtuális hálózat, amely tartalmazza a több alhálózattal, beleértve egy előtér- és háttér-alhálózatot.
* Az Active Directory-tartomány a tartományvezérlő.
* Két SQL Server virtuális gépen telepített toohello háttér-alhálózathoz és Active Directory-tartományhoz csatlakoztatott toohello.
* Egy három csomópontos Windows feladatátvevő fürt hello csomóponttöbbség kvórum modell.
* Egy rendelkezésre állási csoportban két szinkron véglegesítésű másodpéldányt a rendelkezésre állási adatbázis.

Ebben a forgatókönyvben a jó választás az egyszerűség érdekében az Azure-on, nem pedig a költséghatékonyság vagy egyéb tényezők. Például minimalizálhatja hello a két-replika rendelkezésre állási csoport toosave a virtuális gépek száma a számítási órák az Azure-ban hello tartományvezérlő hello kvórum tanúsító fájlmegosztást egy két csomópontos feladatátvevő fürt segítségével. Ez a módszer egy, a fenti konfigurációs hello csökkenti a hello virtuális gépek száma.

Ez az oktatóanyag célja, akkor hello lépéseket, amelyek be hello szükséges tooset tooshow megoldást a fenti leírt hello részletei minden egyes lépést kidolgozása nélkül. Ezért ahelyett, hogy nyújtó hello grafikus felhasználói Felülettel konfigurációs lépések, használja a PowerShell parancsfájl-kezelési tootake meg gyorsan a minden egyes lépést. Ez az oktatóanyag azt feltételezi, hogy a következő hello:

* Már van Azure-fiókot hello virtuálisgép-előfizetés.
* Hello telepítése [Azure PowerShell-parancsmagok](/powershell/azure/overview).
* Már van egy Always On rendelkezésre állási csoportok a helyszíni megoldások alapos ismerete. További információkért lásd: [Always On rendelkezésre állási csoportok (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

## <a name="connect-tooyour-azure-subscription-and-create-hello-virtual-network"></a>Csatlakozás Azure-előfizetés tooyour és hello virtuális hálózat létrehozása
1. Egy PowerShell-ablakban a helyi számítógépen hello Azure modul importálása, töltse le a közzétételi beállítások fájl tooyour gép hello és hello letöltött közzétételi beállítások importálása a PowerShell-munkamenet tooyour Azure-előfizetés kapcsolódnak.

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    Hello **Get-AzurePublishSettingsFile** parancs automatikusan az Azure felügyeleti tanúsítványt hoz létre, és letölti, tooyour gép. A böngésző automatikusan megnyílik, és felszólító tooenter hello Microsoft-fiók hitelesítő adataival Azure-előfizetése. letöltött hello **.publishsettings** fájl tartalmazza az összes hello szükséges információk toomanage az Azure-előfizetéshez. A fájl tooa helyi könyvtár mentése, után importálnia hello **Import-AzurePublishSettingsFile** parancsot.

   > [!NOTE]
   > hello .publishsettings fájlt tartalmaz a hitelesítő adatokat (kódolatlan) használt tooadminister, az Azure-előfizetések és -szolgáltatásokra. hello biztonsági szempontból ajánlott a fájl toostore azt ideiglenesen kívül a forrás-könyvtárak (például a hello Libraries\Documents mappában), majd törölje hello importálása után. Egy rosszindulatú felhasználó kap hozzáférést toohello .publishsettings fájlt szerkesztése, létrehozása és törlése az Azure-szolgáltatások.

2. Adja meg, hogy azt ismertetjük toocreate a felhőalapú informatikai infrastruktúra változókat.

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    Nagy figyelmet toohello tooensure, hogy a parancsok később lesz sikeres, a következő:

   * Változók **$storageAccountName** és **$dcServiceName** egyedinek kell lennie, mert használt tooidentify a felhőalapú társzolgáltatás fiókja és a felhő server, a hello Internet.
   * nevek, amennyit megadott a változók hello **$affinityGroupName** és **$virtualNetworkName** hello virtuális hálózati konfiguráció dokumentum később fogjuk vannak konfigurálva.
   * **$sqlImageName** , amely tartalmazza az SQL Server 2012 Service Pack 1 Enterprise Edition hello Virtuálisgép-lemezkép frissítése hello nevét adja meg.
   * Az egyszerűség kedvéért **Contoso! 000** azonos hello hello teljes oktatóprogram során használt jelszót.

3. Affinitáscsoport létrehozásához.

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

4. Virtuális hálózat létrehozása a konfigurációs fájl importálásával.

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    hello konfigurációs fájl tartalmazza a következő XML-dokumentum hello. Röviden, adja meg a virtuális hálózat néven **ContosoNET** nevű hello affinitáscsoportban **ContosoAG**. Hello címterület rendelkezik **10.10.0.0/16** és két alhálózat **10.10.1.0/24** és **10.10.2.0/24**, amelyeket hello első alhálózat és hátsó alhálózati, illetve. hello első alhálózat akkor helyezheti el ügyfélalkalmazásokat, például a Microsoft SharePoint. hello hátsó alhálózat, ahol helyének hello SQL Server virtuális gépen. Ha módosítja a hello **$affinityGroupName** és **$virtualNetworkName** változók korábban is módosítania kell az alábbi neveket megfelelő hello.

        <NetworkConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

5. Hozzon létre egy tárfiókot, hello affinitáscsoport hozott létre, és állítsa be aktuális tárfiók hello az előfizetéshez társított.

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

6. Hello új felhőalapú szolgáltatás és a rendelkezésre állási készlet létrehozása a hello tartományvezérlő kiszolgálón.

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    Ezek a parancsok védőeszközön dolgok következő hello:

   * **Új AzureVMConfig** hoz létre a Virtuálisgép-konfiguráció.
   * **Adja hozzá AzureProvisioningConfig** által biztosított hello egy különálló Windows Server konfigurációs paramétereket.
   * **Adja hozzá AzureDataDisk** szeretné használni a adattárolás Active Directory, a beállítás set tooNone gyorsítótárazás hello hello adatlemezt ad hozzá.
   * **Új AzureVM** hoz létre egy új felhőalapú szolgáltatás, és hoz létre új Azure virtuális gép hello új felhőszolgáltatásban hello.

7. Hello teljesen kiépítve új virtuális gép toobe várja meg, és töltse le a távoli asztali fájl tooyour munkakönyvtár hello. Mert a hello új Azure virtuális gép egy hosszú ideig tooprovision, hello `while` hurkot továbbra is toopoll hello új virtuális Gépet, amíg a használatra kész.

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

most már sikeresen kiépítette hello tartományvezérlő kiszolgálón. A következő hello Active Directory-tartományhoz kell majd konfigurálnia a tartomány a tartományvezérlő kiszolgálón. Hagyja hello PowerShell ablakban nyissa meg a helyi számítógépen. Azt ismertetjük, hogy újra újabb toocreate hello két SQL Server virtuális gépen.

## <a name="configure-hello-domain-controller"></a>Hello tartományvezérlő konfigurálása
1. Kapcsolódás toohello tartományvezérlő kiszolgálót hello távoli asztali fájl megnyitása. Hello machine felügyeleti AzureAdmin felhasználónévvel és jelszóval **Contoso! 000**, amely létrehozásakor adott új virtuális gép hello.
2. Nyisson meg egy PowerShell-ablakot rendszergazdai módban.
3. Futtassa a következő hello **DCPROMO. EXE** parancs tooset mentése hello **corp.contoso.com** tartományhoz a hello adatkönyvtárak M-meghajtón.

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    Hello parancs befejezése után a virtuális gép hello automatikusan újraindul.

4. Csatlakozzon újra a tartományvezérlő kiszolgálót toohello hello távoli asztali fájl indításával. Most, jelentkezzen be a **Corp/rendszergazda**.
5. Nyisson meg egy PowerShell-ablakot rendszergazdai módban, és hello Active Directory PowerShell-modul importálása a következő parancs hello segítségével:

        Import-Module ActiveDirectory

6. Futtassa a következő parancsok tooadd három felhasználók toohello tartomány hello.

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    **CORP\Install** van használt tooconfigure minden kapcsolódó toohello SQL kiszolgáló-szolgáltatáspéldány, a hello feladatátvevő fürt és a hello rendelkezésre állási csoporthoz. **CORP\SQLSvc1** és **CORP\SQLSvc2** hello két SQL Server virtuális gépen hello SQL Server szolgáltatás fiókok használhatók.
7. Hello következő, futtassa a következő parancsok toogive **CORP\Install** hello engedélyek toocreate számítógép-objektumok hello tartományban.

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    a fent megadott GUID hello hello GUID hello számítógép objektumtípus. Hello **CORP\Install** fiókot kell hello **összes tulajdonság olvasása** és **számítógép-objektumok létrehozása** engedély toocreate hello aktív közvetlen hello feladatátvételi objektumokhoz fürt. Hello **összes tulajdonság olvasása** így nem kell toogrant engedély már kap tooCORP\Install alapértelmezés szerint az explicit módon. További információk az engedélyeket, amelyek toocreate hello feladatátvevő fürt szükséges, a következő témakörben: [feladatátvevő fürt részletes útmutató: fiókok konfigurálása az Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).

    Most, hogy befejezte az Active Directory és a felhasználói objektumok hello, akkor hozhat létre két SQL Server virtuális gépen, és csatlakoztassa azokat toothis tartomány.

## <a name="create-hello-sql-server-vms"></a>Hello SQL Server virtuális gépek létrehozása
1. Továbbra is toouse hello PowerShell ablak, amely meg van nyitva, a helyi számítógépen. Adja meg a következő további változókat hello:

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    IP-cím hello **10.10.0.4** általában hozzá van rendelve a toohello hello a létrehozott első virtuális gép **10.10.0.0/16** alhálózat az Azure virtuális hálózat. Ellenőrizze, hogy ez az a tartományvezérlő kiszolgálót hello címe futtatásával **IPCONFIG**.
2. Először a következő védőeszközön parancsok toocreate hello futtatási hello hello feladatátvevő fürtben, a virtuális gép nevű **ContosoQuorum**:

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    Vegye figyelembe a fenti hello parancs vonatkozó hello következőket:

   * **Új AzureVMConfig** hoz létre egy Virtuálisgép-konfiguráció hello kívánt rendelkezésre állási készlet neve. hello további virtuális gépek jön létre hello azonos rendelkezésre állási csoport neve, így azok van csatlakoztatva toohello azonos rendelkezésre állási csoportot.
   * **Adja hozzá AzureProvisioningConfig** illesztések hello VM toohello Active Directory-tartományban létrehozott.
   * **Set-AzureSubnet** helyek VM hello hello hátsó alhálózaton.
   * **Új AzureVM** hoz létre egy új felhőalapú szolgáltatás, és hoz létre új Azure virtuális gép hello új felhőszolgáltatásban hello. Hello **DnsSettings** paraméter határozza meg, hogy hello DNS-kiszolgáló, a hello hello új felhőalapú szolgáltatás kiszolgálója hello IP-címmel rendelkezik **10.10.0.4**. Ez az IP-címe hello hello tartományvezérlő kiszolgálón. Ez a paraméter szükséges tooenable sikeresen hello az új virtuális gépek hello cloud service toojoin toohello Active Directory tartományban. Ez a paraméter nélkül manuálisan be kell hello IPv4-beállításokkal a virtuális gép toouse hello tartományvezérlő kiszolgálót hello elsődleges DNS-kiszolgálóként után hello virtuális gép ki van építve, és majd a hello VM toohello Active Directory-tartományhoz csatlakozzon.
3. Futtatási hello következő védőeszközön parancsok toocreate hello SQL Server virtuális gépen, nevű **ContosoSQL1** és **ContosoSQL2**.

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    Vegye figyelembe a fenti hello parancsok vonatkozó hello következőket:

   * **Új AzureVMConfig** használ hello rendelkezésre állási készlet neve megegyezik a hello tartományvezérlő kiszolgálót, és használja az SQL Server 2012 Service Pack 1 Enterprise Edition-lemezképet hello virtuálisgép-katalógus hello. Operációs rendszer lemez tooread gyorsítótárazás csak (nincs írási gyorsítótárazást) hello állítja. Azt javasoljuk, hogy az áttelepített hello adatbázis fájlok tooa külön adatlemez, hogy csatolása toohello virtuális gép, és állítson be nincs olvasási vagy írási gyorsítótárazás. Hello következő ajánlott dolog azonban tooremove írási gyorsítótárazást az hello operációsrendszer-lemez, mert nem távolítható el, olvassa el hello operációsrendszer-lemez gyorsítótárazás.
   * **Adja hozzá AzureProvisioningConfig** illesztések hello VM toohello Active Directory-tartományban létrehozott.
   * **Set-AzureSubnet** helyek VM hello hello hátsó alhálózaton.
   * **Adja hozzá AzureEndpoint** beveszi hozzáférés végpontok, így az ügyfélalkalmazások férhetnek hozzá a hello Internet ezeket az SQL Server services-példányok. Különböző portok tooContosoSQL1 és ContosoSQL2 kap.
   * **Új AzureVM** hoz létre új SQL Server virtuális gép hello a hello ugyanaz a felhőalapú szolgáltatás, ContosoQuorum. Be kell jelölnie hello virtuális gépek ugyanazt a felhőalapú szolgáltatás, ha azt szeretné, a toobe hello hello azonos rendelkezésre állási csoportot.
4. Várjon, amíg minden virtuális gép toobe teljesen kiépítve pedig minden virtuális gép toodownload a távoli asztali fájl tooyour működő könyvtárba. Hello `for` hurok váltás hello három új virtuális gépek és hello legfelső szintű kerek zárójeleket belül hello parancsok végrehajtása a esetében.

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until hello VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    most már kiépített hello SQL Server virtuális gépen, és fut, de van telepítve az SQL Server alapértelmezett beállításokkal.

## <a name="initialize-hello-failover-cluster-vms"></a>Hello feladatátvevő fürt virtuális gépek inicializálása
Ebben a szakaszban kell toomodify hello három kiszolgálót, amelyen le fogja hello feladatátvevő fürt és hello SQL Server telepítése. Konkrétan:

* Minden kiszolgáló: tooinstall hello kell **feladatátvételi fürtszolgáltatás** szolgáltatás.
* Minden kiszolgáló: tooadd kell **CORP\Install** hello gépként **rendszergazda**.
* ContosoSQL1 és ContosoSQL2 csak: tooadd kell **CORP\Install** , egy **sysadmin** hello alapértelmezett adatbázis szerepköréhez.
* ContosoSQL1 és ContosoSQL2 csak: tooadd kell **NT AUTHORITY\System** , a bejelentkezés az alábbi engedélyek használata hello:

  * Az ALTER bármely rendelkezésre állási csoport
  * Csatlakozás SQL
  * Kiszolgáló állapotának megtekintése
* ContosoSQL1 és ContosoSQL2 csak: hello **TCP** protokoll már engedélyezve van az SQL Server virtuális gép hello. Azonban továbbra is szükség tooopen hello tűzfal az SQL Server távoli hozzáféréshez.

Most hamarosan kész toostart. Kezdve **ContosoQuorum**, kövesse az alábbi hello lépéseket:

1. Csatlakozás túl**ContosoQuorum** hello távoli asztali fájlok indításával. Adjon hello gép rendszergazdai jogosultságú felhasználónevet **AzureAdmin** és a jelszó **Contoso! 000**, a megadott hello virtuális gépek létrehozásakor.
2. Győződjön meg arról, hogy hello számítógépek rendelkeznek sikeresen csatlakoztatva lett túl**corp.contoso.com**.
3. Várjon, amíg hello SQL Server telepítési toofinish futó automatikus hello inicializálási feladat folytatása előtt.
4. Nyisson meg egy PowerShell-ablakot rendszergazdai módban.
5. Hello Windows feladatátvételi fürtszolgáltatás telepítése.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. Adja hozzá **CORP\Install** helyi rendszergazdaként.

        net localgroup administrators "CORP\Install" /Add
7. Jelentkezzen ki ContosoQuorum. Befejezte a kiszolgáló most.

        logoff.exe

A következő inicializálása **ContosoSQL1** és **ContosoSQL2**. Kövesse hello lépéseket, mindkét SQL Server virtuális gépek azonos.

1. Kapcsolódás toohello két SQL Server VMs hello távoli asztali fájlok megnyitása. Adjon hello gép rendszergazdai jogosultságú felhasználónevet **AzureAdmin** és a jelszó **Contoso! 000**, a megadott hello virtuális gépek létrehozásakor.
2. Győződjön meg arról, hogy hello számítógépek rendelkeznek sikeresen csatlakoztatva lett túl**corp.contoso.com**.
3. Várjon, amíg hello SQL Server telepítési toofinish futó automatikus hello inicializálási feladat folytatása előtt.
4. Nyisson meg egy PowerShell-ablakot rendszergazdai módban.
5. Hello Windows feladatátvételi fürtszolgáltatás telepítése.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. Adja hozzá **CORP\Install** helyi rendszergazdaként.

        net localgroup administrators "CORP\Install" /Add
7. Importálás hello SQL-kiszolgáló PowerShell-szolgáltatóban.

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking
8. Adja hozzá **CORP\Install** , hello SysAdmin (rendszergazda) szerepkör hello alapértelmezett SQL Server-példányhoz.

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."
9. Adja hozzá **NT AUTHORITY\System** , a bejelentkezés a fent leírt három hello engedélyekkel.

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP too[NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL too[NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE too[NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
10. Nyissa meg az SQL Server távoli hozzáféréshez hello tűzfalat.

         netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP
11. Jelentkezzen ki mindkét virtuális gépeket.

         logoff.exe

Végül hamarosan kész tooconfigure hello rendelkezésre állási csoport. Hello SQL-kiszolgáló PowerShell-szolgáltatóban tooperform összes hello működik, a használt **ContosoSQL1**.

## <a name="configure-hello-availability-group"></a>Hello rendelkezésre állási csoport konfigurálása
1. Csatlakozás túl**ContosoSQL1** újra hello távoli asztali fájlok indításával. Hello gép fiókkal történő bejelentkezés helyett használatával írja alá **CORP\Install**.
2. Nyisson meg egy PowerShell-ablakot rendszergazdai módban.
3. Adja meg a következő változók hello:

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"
4. Importálás hello SQL-kiszolgáló PowerShell-szolgáltatóban.

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking
5. Hello SQL Server szolgáltatás fiókjának ContosoSQL1 tooCORP\SQLSvc1 módosítása.

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
6. Hello SQL Server szolgáltatás fiókjának ContosoSQL2 tooCORP\SQLSvc2 módosítása.

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
7. Töltse le **CreateAzureFailoverCluster.ps1** a [feladatátvevő fürt létrehozása az Always On rendelkezésre állási csoportok Azure virtuális gép](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) toohello helyi munkakönyvtár. A parancsfájl toohelp működő feladatátvevő fürt létrehozása fogja használni. Fontos információk a Windows feladatátvételi fürtszolgáltatás és együttműködését hello Azure hálózati, lásd: [az SQL Server Azure virtuális gépek magas rendelkezésre állási és vészhelyreállítási helyreállítási](../sql/virtual-machines-windows-sql-high-availability-dr.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).
8. Tooyour munkakönyvtár megváltoztatására, és hozzon létre hello feladatátvevő fürt letöltött hello parancsfájl.

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"
9. Engedélyezze az Always On rendelkezésre állási csoportok hello alapértelmezett SQL Server-példányok a **ContosoSQL1** és **ContosoSQL2**.

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
10. Hozzon létre egy biztonsági mentési könyvtárat, és adja meg az SQL Server szolgáltatásfiókok hello engedélyeket. A könyvtár tooprepare hello rendelkezésre állási adatbázis hello másodlagos másodpéldányon fogja használni.

         $backup = "C:\backup"
         New-Item $backup -ItemType directory
         net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
         icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")
11. Adatbázis létrehozása a **ContosoSQL1** nevű **MyDB1**, teljes biztonsági mentés és a napló biztonsági mentését is igénybe vehet, és visszaállítja ezeket **ContosoSQL2** a hello **WITH NORECOVERY** lehetőséget.

         Invoke-SqlCmd -Query "CREATE database $db"
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery
12. Az SQL Server VMs hello hello rendelkezésre állási csoport végpontokat hoz létre, és adja meg a hello megfelelő engedélyeket hello végpontokon.

         $endpoint =
             New-SqlHadrEndpoint MyMirroringEndpoint `
             -Port 5022 `
             -Path "SQLSERVER:\SQL\$server1\Default"
         Set-SqlHadrEndpoint `
             -InputObject $endpoint `
             -State "Started"
         $endpoint =
             New-SqlHadrEndpoint MyMirroringEndpoint `
             -Port 5022 `
             -Path "SQLSERVER:\SQL\$server2\Default"
         Set-SqlHadrEndpoint `
             -InputObject $endpoint `
             -State "Started"

         Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] too[$acct2]" -ServerInstance $server1
         Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] too[$acct1]" -ServerInstance $server2
13. Hozzon létre hello rendelkezésre állási másodpéldányok.

         $primaryReplica =
             New-SqlAvailabilityReplica `
             -Name $server1 `
             -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
             -AvailabilityMode "SynchronousCommit" `
             -FailoverMode "Automatic" `
             -Version 11 `
             -AsTemplate
         $secondaryReplica =
             New-SqlAvailabilityReplica `
             -Name $server2 `
             -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
             -AvailabilityMode "SynchronousCommit" `
             -FailoverMode "Automatic" `
             -Version 11 `
             -AsTemplate
14. Végezetül hozza létre a hello rendelkezésre állási csoport és a csatlakozás hello másodlagos replika toohello a rendelkezésre állási csoporthoz.

         New-SqlAvailabilityGroup `
             -Name $ag `
             -Path "SQLSERVER:\SQL\$server1\Default" `
             -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
             -Database $db
         Join-SqlAvailabilityGroup `
             -Path "SQLSERVER:\SQL\$server2\Default" `
             -Name $ag
         Add-SqlAvailabilityDatabase `
             -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
             -Database $db

## <a name="next-steps"></a>Következő lépések
Ön már most sikeresen megvalósítva SQL Server Always On rendelkezésre állási csoport létrehozása az Azure-ban. a rendelkezésre állási csoport egy figyelő tooconfigure lásd [egy ILB figyelőt az Always On rendelkezésre állási csoportok konfigurálása az Azure-](../classic/ps-sql-int-listener.md).

Egyéb Azure-ban az SQL Server használatával kapcsolatos információkért lásd: [Azure virtuális gépeken futó SQL Server](../sql/virtual-machines-windows-sql-server-iaas-overview.md).
