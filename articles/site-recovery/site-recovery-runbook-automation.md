---
title: aaaAdd Azure Automation runbookjai toorecovery tervek az Azure Site Recovery |} Microsoft Docs
description: "Ismerje meg, hogyan Azure Site Recovery segítségével a helyreállítási terv kiterjesztése az Azure Automation használatával. Ismerje meg, hogyan toocomplete összetett feladatok, helyreállítási tooAzure során."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: ecece14d-5f92-4596-bbaf-5204addb95c2
ms.service: site-recovery
ms.devlang: powershell
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 06/23/2017
ms.author: ruturajd@microsoft.com
ms.openlocfilehash: 90d517200cec5527e98a0d00da466bace587b70b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-azure-automation-runbooks-toorecovery-plans"></a>Azure Automation runbookjai toorecovery csomagok hozzáadása
Ez a cikk azt ismertetik, hogyan integrálható az Azure Site Recovery Azure Automation toohelp kiterjeszti a helyreállítási terv. A helyreállítási terv lehet levezényelni a Site Recovery védett virtuális gépek helyreállítása. A helyreállítási terv replikációs tooa másodlagos felhő, valamint az replikációs tooAzure működik. A helyreállítási terv is segít hello helyreállítási **következetesen pontos**, **ismételhető**, és **automatizált**. Ha a rendszer átadja a virtuális gépek tooAzure, integráció az Azure Automation szolgáltatásban, a helyreállítási terv terjeszti ki. Használat tooexecute runbookok, amelyek hatékony automatizálási feladatai kiindulópontjaként.

Ha új tooAzure Automation, akkor [regisztráljon](https://azure.microsoft.com/services/automation/) és [mintaparancsfájlok letöltése](https://azure.microsoft.com/documentation/scripts/). További információk és toolearn hogyan tooorchestrate helyreállítási tooAzure használatával [helyreállítási tervek](https://azure.microsoft.com/blog/?p=166264), lásd: [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/).

Ez a cikk azt ismerteti hogyan integrálható az Azure Automation-forgatókönyv be a helyreállítási terv. Példák tooautomate alapvető műveleteket, amelyek korábban a kézi beavatkozás szükséges használjuk. Azt is leírják, hogyan tooconvert egy többlépéses helyreállítási tooa kattintással helyreállítási művelet.

## <a name="customize-hello-recovery-plan"></a>Hello helyreállítási terv testreszabása
1. Nyissa meg toohello **Site Recovery** helyreállítási terv erőforráspanelen. Az ebben a példában a hello helyreállítási tervben szereplő két virtuális gépek hozzáadott tooit, a helyreállításhoz. egy runbook hozzáadása toobegin kattintson hello **Testreszabás** fülre.

    ![Hello Testreszabás gombra.](media/site-recovery-runbook-automation-new/essentials-rp.png)


2. Kattintson a jobb gombbal **csoport 1: Start**, majd válassza ki **post művelet hozzáadása**.

    ![Kattintson a jobb gombbal csoport 1: Start, és a post művelet hozzáadása](media/site-recovery-runbook-automation-new/customize-rp.png)

3. Kattintson a **válasszon egy parancsfájlt**.

4. A hello **műveletének frissítése** panelen, hello parancsfájlt **Hello World**.

    ![hello frissítési művelet panel](media/site-recovery-runbook-automation-new/update-rp.png)

5. Adja meg az Automation-fiók nevét.
    >[!NOTE]
    > Automation-fiók hello bármely Azure régióban lehet. Automation-fiók hello hello kell lennie és hello Azure Site Recovery-tárolónak ugyanabban az előfizetésben.

6. Az Automation-fiók válasszon egy runbookot. Ez a runbook végrehajtásakor hello hello helyreállítási terv, hello első csoport hello helyreállítás után futó hello parancsfájl.

7. toosave hello parancsfájl, kattintson a **OK**. hello parancsfájl túl kerül**csoport 1: utáni lépéseket**.

    ![Követő műveletet csoport 1:Start](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="considerations-for-adding-a-script"></a>A parancsfájl hozzáadása szempontjai

* A beállítások megtekintéséhez túl**egy lépés törlése** vagy **hello parancsfájl frissítése**, kattintson a jobb gombbal a hello parancsfájl.
* A parancsfájl futtatásához Azure feladatátvétel során a egy a helyszíni gépen tooAzure. Is futtatható Azure leállítás, mielőtt elsődleges webhelyeken parancsfájlként Azure tooan a helyi számítógépen, a feladat-visszavétel során.
* Parancsfájl futtatása, a helyreállítási terv környezet esetében. hello a következő példa bemutatja, egy környezeti változó:

    ```
            {"RecoveryPlanName":"hrweb-recovery",

            "FailoverType":"Test",

            "FailoverDirection":"PrimaryToSecondary",

            "GroupId":"1",

            "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                    { "SubscriptionId":"7a1111111-c1d6-49c5-8c5d-111ce8dd183",

                    "ResourceGroupName":"ContosoRG",

                    "CloudServiceName":"pod02hrweb-Chicago-test",

                    "RoleName":"Fabrikam-Hrweb-frontend-test",

                    "RecoveryPointId":"TimeStamp"}

                    }

            }
    ```

    hello a következő táblázat felsorolja a hello nevét és leírását mindegyik változónak hello a környezetben.

    | **Változó neve** | **Leírás** |
    | --- | --- |
    | RecoveryPlanName |hello neve hello terv futtatása. Ez a változó segítségével különböző műveletek hello helyreállítási terv neve alapján. Hello parancsfájlt is felhasználhatja. |
    | FailoverType |Megadja, hogy hello feladatátvételi teszt, tervezett vagy nem tervezett. |
    | FailoverDirection |Megadja, hogy helyreállítási tooa elsődleges vagy másodlagos helyhez. |
    | Csoportazonosító |Hello csoportazonosító azonosítja hello helyreállítási terv hello terv fut-e. |
    | VmMap |Hello csoportban lévő összes virtuális gép tömbjét. |
    | VMMap kulcs |Egyedi kulcs (GUID) az egyes virtuális gépek. Fennállt – azonos Azure Virtual Machine Manager (VMM) azonosítója hello hello hello a virtuális gép, szükség szerint. |
    | SubscriptionId |hello Azure-előfizetése Azonosítóját, amelyben a virtuális gép hello hozták létre. |
    | RoleName |hello Azure virtuális Gépen, amelyik a helyreállítandó hello neve. |
    | CloudServiceName |hello Azure felhőalapú szolgáltatás neve alapján, amely a virtuális gép hello hozták létre. |
    | erőforráscsoport-név|hello Azure erőforráscsoport-név alapján mely hello VM lett létrehozva. |
    | RecoveryPointId|Ha a virtuális gép hello helyre lett állítva hello időbélyegzőjét. |

* Győződjön meg arról, hogy hello Automation-fiók rendelkezik a következő modulok hello:
    * AzureRM.profile
    * AzureRM.Resources
    * AzureRM.Automation
    * AzureRM.Network
    * AzureRM.Compute

Minden modul kompatibilis verziók kell lennie. Egy egyszerű módot tooensure kompatibilis összes modulokra toouse hello a legfrissebb verziója minden hello modulok.

### <a name="access-all-vms-of-hello-vmmap-in-a-loop"></a>Minden virtuális gépeinek hello VMMap ismétlődő eléréséhez
Kód tooloop hello Microsoft VMMap az összes virtuális gépek között a következő hello használata:

```
$VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
$vmMap = $RecoveryPlanContext.VmMap
 foreach($VMID in $VMinfo)
 {
     $VM = $vmMap.$VMID                
             if( !(($VM -eq $Null) -Or ($VM.ResourceGroupName -eq $Null) -Or ($VM.RoleName -eq $Null))) {
         #this check is tooensure that we skip when some data is not available else it will fail
 Write-output "Resource group name ", $VM.ResourceGroupName
 Write-output "Rolename " = $VM.RoleName
     }
 }

```

> [!NOTE]
> hello erőforrás csoport nevét és a szerepkör neve értékek nincsenek megadva, ha hello parancsfájl egy előtti művelet tooa rendszerindítási csoport. hello értékek a rendszer feltölti, csak akkor, ha hello az adott csoport a virtuális gép feladatátvételi sikeres lesz. hello parancsfájl hello rendszerindítási csoport tagjai a követő művelet.

## <a name="use-hello-same-automation-runbook-in-multiple-recovery-plans"></a>Használja az azonos hello több helyreállítási tervek az Automation-forgatókönyv

A több helyreállítási tervek egy parancsfájl külső változók segítségével is használhatók. Használhat [Azure Automation-változók](../automation/automation-variables.md) toostore paramétereket, amelyek átadhatók egy helyreállítási terv végrehajtása. Adja hozzá a hello helyreállítási terv neve előtag toohello változóként, minden helyreállítási terv egyes változók is létrehozhat. Ezt követően változókkal hello paraméterekként. Módosíthatja egy paraméter hello parancsfájl módosítása nélkül, de továbbra is módosítás hello módon hello parancsfájl.

### <a name="use-a-simple-string-variable-in-a-runbook-script"></a>Használhat egy egyszerű karakterlánc változót egy runbook parancsfájl

Ebben a példában egy parancsfájlt a hálózati biztonsági csoport (NSG) hello bemenetből fogad adatokat, és alkalmazza azt toohello virtuális gépeket a helyreállítási terv.

Hello parancsfájl toodetect melyik helyreállítási tervhez fut, a hello helyreállítási terv környezet használata:

```
workflow AddPublicIPAndNSG {
    param (
          [parameter(Mandatory=$false)]
          [Object]$RecoveryPlanContext
    )

    $RPName = $RecoveryPlanContext.RecoveryPlanName
```

egy meglévő NSG tooapply, ismernie kell hello NSG neve és hello NSG az erőforráscsoport neve. Ezek a változók használják bemeneti adatokat a helyreállítási terv parancsprogramokat. toodo, hozzon létre két változót hello az Automation-fiók eszközök. Adja hozzá a hello nevét hello helyreállítási terv létrehozása hello paramétereinek előtag toohello változó neveként.

1. Hozzon létre egy változó toostore hello NSG neve. Vegyen fel egy előtag toohello változónevet hello helyreállítási terv hello neve.

    ![Az NSG neve változó létrehozása](media/site-recovery-runbook-automation-new/var1.png)

2. Hozzon létre egy változó toostore hello NSG az erőforráscsoport neve. Vegyen fel egy előtag toohello változónevet hello helyreállítási terv hello neve.

    ![Hozzon létre egy NSG-t az erőforráscsoport neve](media/site-recovery-runbook-automation-new/var2.png)


3.  Hello parancsfájlban használja a következő hivatkozás kód tooget hello változók értékeinek hello:

    ```
    $NSGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSG"
    $NSGRGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSGRG"

    $NSGnameVar = Get-AutomationVariable -Name $NSGValue
    $RGnameVar = Get-AutomationVariable -Name $NSGRGValue
    ```

4.  Hello runbook tooapply hello NSG toohello hálózati adapterének hello használata hello változók átvevő VM:

    ```
    InlineScript {
    if (($Using:NSGname -ne $Null) -And ($Using:NSGRGname -ne $Null)) {
            $NSG = Get-AzureRmNetworkSecurityGroup -Name $Using:NSGname -ResourceGroupName $Using:NSGRGname
            Write-output $NSG.Id
            #Apply hello NSG tooa network interface
            #$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
            #Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
            #  -AddressPrefix 192.168.1.0/24 -NetworkSecurityGroup $NSG
        }
    }
    ```

Minden helyreállítási terv hozza létre a független változók hello parancsfájlt is felhasználhatja. Egy előtagot hello helyreállítási terv nevének megadásával. Ebben a forgatókönyvben egy teljes, végpontok közötti parancsfájlt talál [a Site Recovery helyreállítási terv teszt feladatátvétele során adja hozzá a nyilvános IP-cím és NSG tooVMs](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee).


### <a name="use-a-complex-variable-toostore-more-information"></a>Használja a komplex változó toostore további információk

Fontolja meg egy olyan forgatókönyvet, amelyben egy adott virtuális gépeken nyilvános IP-egyetlen parancsfájl tooturn szeretné. Más esetben érdemes lehet a tooapply különböző NSG-k (nem az összes virtuális gépeken futó) különböző gépeken. Egy parancsfájl, amely a helyreállítási terv újrafelhasználható végezheti el. Minden helyreállítási terv olyan virtuális gépek száma lehet. Például egy SharePoint helyreállítási két elülső végpontja van. Egy alapszintű az üzletági (LOB) alkalmazás csak egy előtér rendelkezik. Külön változók minden helyreállítási terv nem hozható létre. 

A következő példa hello, hogy egy új módszerrel, és hozzon létre egy [komplex változó](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396) hello Azure Automation-fiók eszközökbe. Ehhez több érték megadásával. A következő lépéseket az Azure PowerShell toocomplete hello kell használnia:

1. PowerShell, a bejelentkezés tooyour Azure-előfizetés:

    ```
    login-azurermaccount
    $sub = Get-AzureRmSubscription -Name <SubscriptionName>
    $sub | Select-AzureRmSubscription
    ```

2. toostore hello paraméterek, hello komplex változó hello neve hello helyreállítási terv létrehozása:

    ```
    $VMDetails = @{"VMGUID"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"};"VMGUID2"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"}}
        New-AzureRmAutomationVariable -ResourceGroupName <RG of Automation Account> -AutomationAccountName <AA Name> -Name <RecoveryPlanName> -Value $VMDetails -Encrypted $false
    ```

3. A komplex változóban **VMDetails** hello Virtuálisgép-azonosító hello a virtuális gép védett. tooget hello virtuális gép azonosítója az Azure-portálon hello hello virtuális gép tulajdonságainak megtekintése. hello következő képernyőfelvétel egy változó, amely tárolja a két virtuális gépek hello részleteit:

    ![Használja a hello GUID hello virtuális gép azonosítója](media/site-recovery-runbook-automation-new/vmguid.png)

4. Ez a változó használata a runbookokban. Hello feltüntetve VM GUID hello helyreállítási tervnek a környezetben található, ha telepíteni hello NSG hello VM:

    ```
    $VMDetailsObj = Get-AutomationVariable -Name $RecoveryPlanContext.RecoveryPlanName
    ```

4. A runbookban ismétlése hello virtuális gépeinek hello helyreállítási tervnek a környezetben. Ellenőrizze, hogy létezik-e virtuális gép hello az **$VMDetailsObj**. Ha létezik, hello változó tooapply hello NSG hello tulajdonságai érhetők el:

    ```
        $VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
        $vmMap = $RecoveryPlanContext.VmMap

        foreach($VMID in $VMinfo) {
            Write-output $VMDetailsObj.value.$VMID

            if ($VMDetailsObj.value.$VMID -ne $Null) { #If hello VM exists in hello context, this will not b Null
                $VM = $vmMap.$VMID
                # Access hello properties of hello variable
                $NSGname = $VMDetailsObj.value.$VMID.'NSGName'
                $NSGRGname = $VMDetailsObj.value.$VMID.'NSGResourceGroupName'

                # Add code tooapply hello NSG properties toohello VM
            }
        }
    ```

Használhatja ugyanazt a parancsfájlt hello különböző helyreállítási tervek. Adjon meg más paraméterekkel hello érték, amely megfelel a különböző változók tooa helyreállítási tervben tárolja.

## <a name="sample-scripts"></a>Mintaszkriptek

toodeploy minta parancsfájlok tooyour Automation-fiókot, kattintson a hello **tooAzure telepítése** gombra.

[![TooAzure telepítése](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

Egy másik példa tekintse meg a következő videó hello. Bemutatja, hogyan toorecover egy kétrétegű WordPress alkalmazás tooAzure:


> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]



## <a name="additional-resources"></a>További források
* [Azure Automation szolgáltatást futtató fiók](../automation/automation-sec-configure-azure-runas-account.md)
* [Azure Automation áttekintése](http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure Automation – áttekintés")
* [Azure Automation-mintaparancsfájlok](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "Azure Automation-mintaparancsfájlok")
