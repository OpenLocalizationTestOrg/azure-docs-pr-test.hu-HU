---
title: "a PowerShell használatával Windows virtuális gép aaaCreate |} Microsoft Docs"
description: "Hozzon létre Windows virtuális gépek Azure PowerShell és a hello klasszikus telepítési modell használatával."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 42c0d4be-573c-45d1-b9b0-9327537702f7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 5339f458c1f08f6e1752a53191f19402fab8ea25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-with-powershell-and-hello-classic-deployment-model"></a>Windows virtuális gép létrehozása a PowerShell és hello klasszikus telepítési modell
> [!div class="op_single_selector"]
> * [Azure portál – Windows](tutorial.md)
> * [PowerShell - Windows](create-powershell.md)
> 
> 

<br>

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Ismerje meg, hogyan túl[hello Resource Manager modellt használja a következő lépésekkel](../../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Ezen lépések bemutatják, hogyan toocustomize Azure PowerShell készlete parancsok, amelyek létrehozása, és előre konfigurálása a Windows-alapú Azure virtuális gép egy építőelem módszer használatával. Ez a folyamat használhat tooquickly létrehozhat egy új Windows-alapú virtuális gép egy parancsot, és bontsa ki a meglévő telepítés vagy egy egyéni fejlesztési és tesztelési célú vagy pro IT-környezet létrehozása több parancs beállítja, hogy gyorsan toocreate.

Ezeket a lépéseket hajtsa végre az Azure PowerShell-parancs készletek létrehozásához a fill-a-a-üres cellákat megközelítést. Ezt a módszert akkor lehet hasznos, ha új tooPowerShell, vagy csak szeretné tooknow milyen értékeket toospecify sikeres konfigurációhoz. Speciális PowerShell felhasználók hello parancsok igénybe, és a hello változók (hello sorok kezdve a "$") a saját értékeit helyettesítse.

Ha még nem tette meg, használja a hello utasításait [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) tooinstall Azure PowerShell, a helyi számítógépen. Ezután nyissa meg a Windows PowerShell-parancssort.

## <a name="step-1-add-your-account"></a>1. lépés: A fiók hozzáadása
1. Hello PowerShell parancssorába írja be a **Add-AzureAccount** kattintson **Enter**. 
2. Írja be az Azure-előfizetéshez társított hello e-mail címét, és kattintson a **Folytatás**. 
3. Adja meg a fiókhoz tartozó jelszót hello. 
4. Kattintson a **bejelentkezés**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>2. lépés: Az előfizetés és a storage-fiók beállítása
Ezek a parancsok futtatásával hello Windows PowerShell parancssorába állítsa be a Azure-előfizetés és a storage-fiók. Cserélje le mindent, ami hello idézőjelek között, beleértve a hello < és > karakter, helyes nevek hello.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

Hello megfelelő előfizetés neve letölthető hello hello hello kimenete SubscriptionName tulajdonságának **Get-AzureSubscription** parancsot. Hello helyes-e a tárfiók nevének beolvasása hello hello hello kimenete Label tulajdonságának **Get-AzureStorageAccount** parancsot, miután lefuttatta a hello **válasszon-AzureSubscription** parancsot.

## <a name="step-3-determine-hello-imagefamily"></a>3. lépés: Hello ImageFamily meghatározása
A következő lépésben toodetermine hello ImageFamily vagy a címkeérték hello azokba megfelelő toohello toocreate kívánt Azure virtuális gép számára. Hello elérhető ImageFamily értékek listáját ezzel a paranccsal kaphat.

    Get-AzureVMImage | select ImageFamily -Unique

Íme néhány példa a Windows-alapú számítógépek ImageFamily értékeket:

* Windows Server 2012 R2 Datacenter
* Windows Server 2008 R2 SP1
* A Windows Server 2016 Technical Preview 4
* SQL Server 2012 SP1 Enterprise, Windows Server 2012

Ha megtalálta a keresett hello kép, nyissa meg a friss példányának hello szövegszerkesztőben a választott vagy hello PowerShell integrált parancsfájlkezelési környezet (ISE). Hello következő hello új szöveges fájlt vagy a PowerShell ISE hello ImageFamily érték és hello másolja.

    $family="<ImageFamily value>"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Bizonyos esetekben hello lemezképnév hello Label tulajdonságának hello ImageFamily érték helyett szerepel. Ha a keresett hello ImageFamily tulajdonsággal hello kép nem található, listában hello képek ezzel a paranccsal a címke tulajdonság által.

    Get-AzureVMImage | select Label -Unique

Ha hello jobb kép ezzel a paranccsal, nyissa meg a friss példánya a választott vagy a PowerShell ISE hello hello szövegszerkesztőben. Hello következő másolása hello új szöveges fájlt vagy a PowerShell ISE hello címkeérték és hello.

    $label="<Label value>"
    $image = Get-AzureVMImage | where { $_.Label -eq $label } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

## <a name="step-4-build-your-command-set"></a>4. lépés: A parancs-készlet létrehozása
A parancs, állítsa be az új szöveges fájlba vagy hello ISE hello változó kitöltése és hello < és > karakter eltávolítása hello megfelelő állít elő alatt másolásával hello részeinek felépítéséhez. Lásd: a két hello [példák](#examples) hello hello végeredményt a képet a jelen cikk végén.

Indítsa el a parancs, válassza ki az egyik alábbi két parancs blokkok (kötelező).

1. lehetőség: Adja meg a virtuális gép nevét és méretét.

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

2. lehetőség: Adja meg a nevét, méretét, és rendelkezésre állási készlet neve.

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $availset="<set name>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image -AvailabilitySetName $availset

Hello InstanceSize D-, DS- vagy értékeit G sorozatú virtuális gépek, lásd: [virtuális gépek és Felhőszolgáltatások mérete az Azure-](https://msdn.microsoft.com/library/azure/dn197896.aspx).

> [!NOTE]
> Ha egy frissítési garancia nagyvállalati szerződéssel, és tootake előnyeit, Windows Server hello kíván [hibrid használata juttatás](https://azure.microsoft.com/pricing/hybrid-use-benefit/), adja hozzá a **- LicenseType** paraméter toohello  **Új AzureVMConfig** parancsmag hello értéket átadja **Windows_Server** a hello tipikus használati eset.  Lehet, hogy a képfájl feltöltött; nem használhat egy szabványos lemezkép hello gyűjteménye hello hibrid használja juttatásra.
> 
> 

Önálló Windows számítógép esetén megadhat hello helyi rendszergazda fiókot és jelszót.

    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

Írjon be egy erős jelszót. toocheck annak erőssége, lásd: [jelszóellenőrző: az erős jelszavak használata](https://www.microsoft.com/security/pc-security/password-checker.aspx).

Tooadd hello Windows számítógép tooan meglévő Active Directory-tartomány, megadhat hello helyi rendszergazdai fiók és jelszó, hello a tartományban, és hello nevét és egy tartományi fiók.

    $cred1=Get-Credential –Message "Type hello name and password of hello local administrator account."
    $cred2=Get-Credential –Message "Now type hello name (not including hello domain) and password of an account that has permission tooadd hello machine toohello domain."
    $domaindns="<FQDN of hello domain that hello machine is joining>"
    $domacctdomain="<domain of hello account that has permission tooadd hello machine toohello domain>"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

További előtti konfigurációs beállítások a Windows-alapú virtuális gépekhez, lásd: hello hello szintaxisának **Windows** és **WindowsDomain és** paraméterkészletek [ Adja hozzá AzureProvisioningConfig](/powershell/module/azure/add-azureprovisioningconfig).

Másik lehetőségként hozzárendelése hello virtuális gép egy adott IP-cím, egy statikus DIP néven.

    $vm1 | Set-AzureStaticVNetIP -IPAddress <IP address>

Ellenőrizheti, hogy az elérhető legyen-e egy adott IP-cím:

    Test-AzureStaticVNetIP –VNetName <VNet name> –IPAddress <IP address>

Másik lehetőségként hozzárendelése hello virtuális gép tooa megadott alhálózat egy Azure virtuális hálózatban.

    $vm1 | Set-AzureSubnet -SubnetNames "<name of hello subnet>"

Opcionálisan adja hozzá a virtuális gép egyetlen lemez toohello.

    $disksize=<size of hello disk in GB>
    $disklabel="<hello label on hello disk>"
    $lun=<Logical Unit Number (LUN) of hello disk>
    $hcaching="<Specify one: ReadOnly, ReadWrite, None>"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

Egy Active Directory-tartományvezérlő beállítása $hcaching túl "None".

Opcionálisan adja hozzá a hello virtuális gép tooan elosztott terhelésű készlet a külső forgalom.

    $protocol="<Specify one: tcp, udp>"
    $localport=<port number of hello internal port>
    $pubport=<port number of hello external port>
    $endpointname="<name of hello endpoint>"
    $lbsetname="<name of hello existing load-balanced set>"
    $probeprotocol="<Specify one: tcp, http>"
    $probeport=<TCP or HTTP port number of probe traffic>
    $probepath="<URL path for probe traffic>"
    $vm1 | Add-AzureEndpoint -Name $endpointname -Protocol $protocol -LocalPort $localport -PublicPort $pubport -LBSetName $lbsetname -ProbeProtocol $probeprotocol -ProbePort $probeport -ProbePath $probepath

Végül válasszon egyet ezek szükséges parancs blokkok hello virtuális gép létrehozásához.

1. lehetőség: Hozzon létre egy meglévő felhőszolgáltatáshoz hello virtuális gép.

    New-AzureVM –ServiceName "<short name of hello cloud service>" -VMs $vm1

hello rövid neve hello felhőalapú szolgáltatás nem hello név jelenik meg a hello listája Felhőszolgáltatások hello Azure-portálon vagy hello hello Azure-portálon az erőforráscsoportok listáját.

2. lehetőség: Hello virtuális gép létrehozása egy létező felhőalapú szolgáltatást és a virtuális hálózat.

    $svcname="<short name of hello cloud service>"
    $vnetname="<name of hello virtual network>"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

## <a name="step-5-run-your-command-set"></a>5. lépés: Futtassa a parancsot
Tekintse át a szövegszerkesztőben parancsfájlkezelő hello Azure PowerShell parancskészlethez vagy hello a PowerShell ISE álló a több blokkra parancsok a 4. lépéssel. Győződjön meg arról, hogy megadta-e minden szükséges hello változót, és hogy hello helyes az értékük. Ellenőrizze azt is, hogy eltávolította-e az összes hello < és > karakter.

Használatakor egy szövegszerkesztőben, a Másolás hello parancs toohello vágólapra beállítása, és kattintson a jobb gombbal a megnyitott Windows PowerShell-parancssort. A PowerShell-parancsok sorozataként hello parancskészlethez ki és hozza létre az Azure virtuális gépet. Alternatív megoldásként futtassa a PowerShell ISE hello beállítása hello parancsot.

Ha létrehozni újra a virtuális gép vagy egy hasonló, akkor a következőket teheti:

* Ez a parancs egy olyan PowerShell-parancsfájlt (*.ps1) állítja mentéséhez.
* Ez a parancs egy Azure Automation-runbook a hello állítja mentéséhez **Automation-fiók** hello Azure-portálon szakasza.

## <a id="examples"></a>Példák
Az alábbiakban a két példa hello lépésekkel fent toobuild Azure PowerShell parancs beállítja, hogy a Windows-alapú Azure virtuális gépek létrehozása.

### <a name="example-1"></a>1. példa
A PowerShell kell parancskészlet toocreate hello kezdeti virtuális gép egy Active Directory-tartományvezérlőhöz, amelyek:

* Hello Windows Server 2012 R2 Datacenter rendszerképet használja.
* AZDC1 hello névvel rendelkezik.
* Egy olyan önálló számítógép.
* Rendelkezik egy további adatlemezt 20 GB.
* Hello statikus IP-címmel 192.168.244.4 rendelkezik.
* Hello BackEnd alhálózathoz hello AZDatacenter virtuális hálózat szerepel.
* Azure-KELJFELJANCSI hello felhőszolgáltatás van.

Ez hello megfelelő Azure PowerShell paranccsal állítsa toocreate a virtuális gépre, az üres sorok közötti valamennyi blokkja olvashatóság érdekében.

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="AZDC1"
    $vmsize="Medium"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

    $vm1 | Set-AzureSubnet -SubnetNames "BackEnd"

    $vm1 | Set-AzureStaticVNetIP -IPAddress 192.168.244.4

    $disksize=20
    $disklabel="DCData"
    $lun=0
    $hcaching="None"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

### <a name="example-2"></a>2. példa
A PowerShell kell parancskészlet toocreate üzleti kiszolgáló virtuális gép, amely:

* Hello Windows Server 2012 R2 Datacenter rendszerképet használja.
* LOB1 hello névvel rendelkezik.
* Hello corp.contoso.com tartomány tagja.
* 200 GB-os egy további adatok lemezzel rendelkezik.
* Hello FrontEnd alhálózathoz hello AZDatacenter virtuális hálózat szerepel.
* Azure-KELJFELJANCSI hello felhőszolgáltatás van.

Ez hello megfelelő Azure PowerShell paranccsal állítsa toocreate ezt a virtuális gépet.

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="LOB1"
    $vmsize="Large"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred1=Get-Credential –Message "Type hello name and password of hello local administrator account."
    $cred2=Get-Credential –Message "Now type hello name (not including hello domain) and password of an account that has permission tooadd hello machine toohello domain."
    $domaindns="corp.contoso.com"
    $domacctdomain="CORP"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "FrontEnd"

    $disksize=200
    $disklabel="LOBData"
    $lun=0
    $hcaching="ReadWrite"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname


## <a name="next-steps"></a>Következő lépések
Ha egy 127 GB-nál nagyobb méretű operációsrendszer-lemez van szüksége, akkor [bontsa ki az operációs rendszer hello meghajtó](../../virtual-machines-windows-expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

