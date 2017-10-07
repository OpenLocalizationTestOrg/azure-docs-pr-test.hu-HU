---
title: "aaaAdd az Azure automation runbookjai toorecovery tervek hello klasszikus portál |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan Azure Site Recovery most már lehetővé teszi tooextend helyreállítási tervek segítségével az Azure Automation toocomplete összetett feladatokat helyreállítási tooAzure során"
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: f24eaa62-9dea-4fce-92e1-a72513ca0289
ms.service: site-recovery
ms.devlang: powershell
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 3bb7420911afbce289b656f28823b1923e8af0c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-azure-automation-runbooks-toorecovery-plans-in-hello-classic-portal"></a>Adja hozzá az Azure automation runbookjai toorecovery tervek hello klasszikus portál
Ez az oktatóanyag leírja, hogyan integrálható az Azure Site Recovery Azure Automation tooprovide bővítési toorecovery tervek. A helyreállítási terv lehet levezényelni a védett replikációs toosecondary felhő- és a replikációs tooAzure forgatókönyvek az Azure Site Recovery segítségével virtuális gépeket a helyreállítási. Hello helyreállítási létrehozása során is segítenek **következetesen pontos**, **ismételhető**, és **automatizált**. A virtuális gépek tooAzure vannak feladatátvétele, ha integráció az Azure Automation szolgáltatásban, terjeszti ki a helyreállítási tervek, és lehetővé teszi az funkció tooexecute runbookok, így hatékony automatizálási feladatok.

Ha Ön már nem hallott az Azure Automation még, iratkozzon fel [Itt](https://azure.microsoft.com/services/automation/) , és töltse le a mintaparancsfájlok [Itt](https://azure.microsoft.com/documentation/scripts/). Tudjon meg többet az [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) , és hogyan tooorchestrate helyreállítási tooAzure helyreállítással tervez [Itt](https://azure.microsoft.com/blog/?p=166264).

Rövid ebben az oktatóanyagban hogyan integrálhatja az Azure Automation-runbook helyreállítási tervek a következő fog keresni. Azt, amely korábban a kézi beavatkozás szükséges egyszerű feladatok automatizálásához, és tekintse meg, hogyan tooconvert egy többszörös lépésenként helyreállítási egy kattintással indítható helyreállítási művelet. Áttekintjük azt is, hogy hogyan háríthatóak el egy egyszerű parancsprogram Ha azt nem megfelelő.

## <a name="protect-hello-application-tooazure"></a>Hello alkalmazás tooAzure védelme
Ossza meg velünk kezdődhet egy egyszerű alkalmazást, amely két virtuális gép. Itt a Fabrikam a HRweb alkalmazás van. Fabrikam-HRweb-előtér- és a Fabrikam-Hrweb-háttérrendszer hello két virtuális gép Azure Site Recovery segítségével tooAzure védett. tooprotect hello virtuális gépek Azure Site Recovery segítségével hajtsa végre az alábbi hello lépéseket.

1. A virtuális gépek védelmének engedélyezése.
2. Győződjön meg arról, hogy hello virtuális gépek kezdeti replikáció befejezése replikálja.
3. Várjon, amíg hello kezdeti replikálása befejeződik, és hello replikációs állapot szerint védett.

## ![](media/site-recovery-runbook-automation/01.png)
Ebben az oktatóanyagban létre fogunk hozni egy helyreállítási terv hello Fabrikam HRweb alkalmazás toofailover hello alkalmazás tooAzure. Majd integrálni fogjuk azt egy runbookhoz, amely létre fog hozni egy olyan végpont a hello Azure virtuális gép tooserve weblapok 80-as porton, a feladatátvételt.

Először hozzon létre egy helyreállítási tervet, az alkalmazás.

## <a name="create-hello-recovery-plan"></a>Hello helyreállítási terv létrehozása
toorecover hello alkalmazás tooAzure kell toocreate helyreállítási tervet.
A helyreállítási terv, megadhatja, hogy a virtuális gépek helyreállítási hello sorrendjének használatával. 1. csoportba kerülnek hello virtuális gép lesz helyreállítására és először, és hello virtuális gép 2 csoportban kövesse.

Hozzon létre egy helyreállítási terv, amely alatt a következőképpen néz.

![](media/site-recovery-runbook-automation/12.png)

További információk a helyreállítási terv, olvassa el a dokumentációt tooread [Itt](https://msdn.microsoft.com/library/azure/dn788799.aspx "Itt").

Következő lépésként hozzon létre hello szükséges összetevők az Azure Automationben.

## <a name="create-hello-automation-account-and-its-assets"></a>Hello automation-fiók és az eszközök létrehozása
Egy Azure Automation-fiók toocreate forgatókönyv van szüksége. Ha még nem rendelkezik fiókkal, keresse meg a tooAzure Automation lapon kimaradásával ![](media/site-recovery-runbook-automation/02.png), és hozzon létre egy új fiókot.

1. Adjon meg egy név tooidentify a hello fiók.
2. Adjon meg egy földrajzi régiót, ahol azt szeretné, hogy tooplace hello fiók.

Ajánlott tooplace hello fiók hello és hello az ASR-tárolónak ugyanabban a régióban.

![](media/site-recovery-runbook-automation/03.png)

Ezután hozzon létre a következő eszközök a fiók hello hello.

### <a name="add-a-subscription-name-as-asset"></a>Adja hozzá egy előfizetés nevét eszköz
1. Egy új beállítással ![](media/site-recovery-runbook-automation/04.png) a hello Azure Automation-eszközök, és válassza ki a túl![](media/site-recovery-runbook-automation/05.png)
2. Válassza ki a változó típusú hello **karakterlánc**
3. Adja meg a változó neve, mint a **AzureSubscriptionName**

   ![](media/site-recovery-runbook-automation/06.png)
4. Adja meg a tényleges Azure-előfizetés neve hello változó értékét.

   ![](media/site-recovery-runbook-automation/07_1.png)

Azonosíthatja, hogy a fiókot az Azure-portálon hello hello beállítások lapján az előfizetés hello nevét.

### <a name="add-an-azure-login-credential-as-asset"></a>Adja hozzá az Azure bejelentkezési hitelesítő adatát eszköz
Azure Automation szolgáltatásbeli Azure PowerShell tooconnect toothe előfizetés használ, és a hello összetevőinek nem működik. Ehhez szüksége hitelesítés használatával a Microsoft-fiókjával vagy a munkahelyi vagy iskolai fiókkal.
Egy eszköz toobe biztonságosan hello runbook által használt hello fiók hitelesítő adatait tárolhatja.

1. Egy új beállítással ![](media/site-recovery-runbook-automation/04.png) a hello Azure Automation-eszközök és kiválasztása![](media/site-recovery-runbook-automation/09.png)
2. Válassza ki a hello hitelesítőadat-típus szerint **Windows PowerShell-hitelesítő adat**
3. Adja meg a hello néven **AzureCredential**

   ![](media/site-recovery-runbook-automation/10.png)
4. Adja meg a hello levő felhasználónevet és jelszót toosign-a.

Most mindkét ezek a beállítások érhetők el az eszközök.

![](media/site-recovery-runbook-automation/11.png)

További információ a PowerShell tooconnect tooyour előfizetésébe kap hogyan [Itt](/powershell/azure/overview).

Ezután létrehoz egy forgatókönyvet az Azure Automationben, hozzáadhat egy végpont hello előtér-virtuális gép a feladatátvételt követően.

## <a name="azure-automation-context"></a>Azure automation-környezet
Az ASR átadja egy környezeti változó toohello runbook toohelp determinisztikus parancsfájlokat írhat. Egy sikerült is, hogy előre jelezhető hello felhőalapú szolgáltatás és a virtuális gép hello hello nevét, de akkor fordul elő, hogy azt nem mindig hello eset toocertain forgatókönyvek használhatók, mint ahol hello neve hello virtuális gép neve lehet, hogy változásai miatt egy hello miatt az Azure-ban toounsupported karaktereket. Ezért ezek az információk átadása toohello automatikus rendszer-Helyreállítás helyreállítási terv hello részeként *környezet*.

Az alábbiakban példája hello környezeti változó megjelenését.

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


hello az alábbi táblázat tartalmazza a nevet és leírást a hello környezetben minden egyes változójánál.

| **Változó neve** | **Leírás** |
| --- | --- |
| RecoveryPlanName |Futtatandó terv nevét. Segítséget nyújt egy nevet adtuk meg művelet elvégzése hello azonos parancsfájl |
| FailoverType |Meghatározza, hogy hello feladatátvételi teszt, tervezett vagy nem tervezett. |
| FailoverDirection |Adja meg, hogy helyreállítási tooprimary vagy másodlagos |
| Csoportazonosító |Azonosítsa a hello csoportszámmal belül hello helyreállítási terv hello csomag futtatásakor |
| VmMap |Hello hello csoportban lévő összes virtuális gép tömbje |
| VMMap kulcs |Egyedi kulcs (GUID) az egyes virtuális gépek. Ugyanaz, mint a hello hello virtuális gép VMM-azonosító, ha alkalmazható rendelkezik hello. |
| RoleName |Hello Azure virtuális Gépen, amelyik a helyreállítandó neve |
| CloudServiceName |Azure Cloud Service neve alapján mely hello a virtuális gép létrehozása. |

tooidentify hello VmMap kulcs hello környezetben is sikerült nyissa meg a virtuális gép tulajdonságlapján toohello az ASR-ben, és nézze meg hello VM GUID tulajdonság.

![](media/site-recovery-runbook-automation/13.png)

## <a name="author-an-automation-runbook"></a>Az Automation-runbook Szerző
Most hozzon létre hello runbook tooopen 80-as port hello előtér virtuális gépen.

1. Hozzon létre egy új runbookot hello Azure Automation-fiók hello nevű **OpenPort80**

   ![](media/site-recovery-runbook-automation/14.png)
2. Keresse meg a toohello hello runbook Szerző nézet, és írja be a hello vázlat módban.
3. Először adja meg a változó toouse hello hello helyreállítási terv környezetben

   ```
       param (
           [Object]$RecoveryPlanContext
       )

   ```
4. Ezután csatlakoztassa toohello feliratkozás hello hitelesítő adatok és az előfizetés neve

   ```
       $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

       # Connect tooAzure
       $AzureAccount = Add-AzureAccount -Credential $Cred
       $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
       Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
   ```

   Vegye figyelembe, hogy használja-e hello Azure eszközök – **AzureCredential** és **AzureSubscriptionName** itt.
5. Most adja meg a hello végpont adatait, és hello hello virtuális gép legyen tooexpose hello végpont GUID. A case hello előtér-virtuális gép.

   ```
       # Specify hello parameters toobe used by hello script
       $AEProtocol = "TCP"
       $AELocalPort = 80
       $AEPublicPort = 80
       $AEName = "Port 80 for HTTP"
       $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
   ```

   Azt határozza meg, hello Azure-végpont protokoll, a virtuális gép hello helyi port és a csatlakoztatott nyilvános port. Ezek a változók hello végpontok tooVMs hozzáadott Azure parancsok paramétereket igényel. hello VMGUID hello hello virtuális gép toooperate kell a GUID érvényes.
6. hello parancsfájl most bontsa ki a virtuális gép GUID adott hello hello környezetben, és hozzon létre egy végpontot, által hivatkozott hello virtuális gépen.

   ```
       #Read hello VM GUID from hello context
       $VM = $RecoveryPlanContext.VmMap.$VMGUID

       if ($VM -ne $null)
       {
           # Invoke pipeline commands within an InlineScript

           $EndpointStatus = InlineScript {
               # Invoke hello necessary pipeline commands tooadd a Azure Endpoint tooa specified Virtual Machine
               # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

               $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                   Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                   Update-AzureVM
               Write-Output $Status
           }
       }
   ```
7. Amikor ez befejeződik, kattintson a közzététel ![](media/site-recovery-runbook-automation/20.png) tooallow a parancsfájl toobe végrehajtási érhető el.

hello teljes parancsfájlt az alábbi táblázat a referenciaként a

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect tooAzure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify hello parameters toobe used by hello script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read hello VM GUID from hello context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke hello necessary pipeline commands tooadd an Azure Endpoint tooa specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## <a name="add-hello-script-toohello-recovery-plan"></a>Hello parancsfájl toohello helyreállítási terv hozzáadása
Ha készen áll a hello parancsfájl, hozzáadhatja azt korábban létrehozott toohello helyreállítási terv.

1. Hello létrehozott helyreállítási csomagot válasszon tooadd parancsfájl után a csoport 2. ![](media/site-recovery-runbook-automation/15.png)
2. Adja meg a parancsfájl nevét. Ez az ehhez a parancsprogramhoz a megjelenítő hello helyreállítási terv belül csak egy rövid nevet.
3. Válassza ki hello feladatátvételi tooAzure parancsfájl – hello Azure Automation-fiók nevét.
4. Azure Runbookokat hello válasszon Ön által létrehozott hello runbookot.

![](media/site-recovery-runbook-automation/16.png)

## <a name="primary-side-scripts"></a>Elsődleges ügyféloldali parancsprogramok
Ha egy feladatátvevő tooAzure állnak végrehajtás alatt, is beállíthatja tooexecute elsődleges ügyféloldali parancsprogramok. Ezek a parancsfájlok a feladatátvétel során hello VMM-kiszolgálón fog futni.
Elsődleges ügyféloldali parancsprogramok csak csak a leállás előtti és utáni leállítási fázisból áll. Ennek oka az, amikor egy olyan vészhelyzet esetén éri általában nem érhető el várhatóan hello elsődleges hely toobe.
Egy nem tervezett feladatátvétel során csak akkor, ha az elsődleges helyen végrehajtott műveletekről csatlakozott a program megpróbálja toorun hello elsődleges ügyféloldali parancsprogramok. Ha nincsenek elérhető-e, vagy időtúllépés, hello feladatátvételi továbbra is toorecover hello virtuális gépeket.
Elsődleges ügyféloldali parancsprogramok esetén VMware vagy fizikai/Hyper-v helyek nélkül védett VMM tooAzure - feladatátvételi tooAzure közben nem érhetők el.
Azonban ha az Azure tooon helyszíni, elsődleges ügyféloldali parancsprogramok (Runbookok) feladatátvételi használható VMware kivételével az összes cél.

## <a name="test-hello-recovery-plan"></a>Tesztelési hello helyreállítási terv
Hello runbook toohello terv hozzáadását követően kezdeményezheti a feladatátvételi tesztet, és a működés ellenőrzése. Mindig ajánlott toorun a teszt feladatátvételi tootest az alkalmazás- és hello helyreállítási terv tooensure, hogy nincsenek-e hibák.

1. Válassza ki a helyreállítási terv hello és a feladatátvételi teszt indíthatnak.
2. Hello terv végrehajtásakor láthatja, hogy hello runbook által végrehajtott, vagy nem keresztül annak állapotát.

   ![](media/site-recovery-runbook-automation/17.png)
3. Azt is láthatja, hello részletes hello Azure Automation feladatok lapon hello runbook runbook végrehajtási állapota.

   ![](media/site-recovery-runbook-automation/18.png)
4. Hello feladatátvételi hello forgatókönyv végrehajtási eredményének, leszámítva befejeződése után megtekintheti a hello végrehajtása sikeres-e, vagy nem a hello Azure virtuális gép meglátogatása, és megnézi a hello végpontok.

![](media/site-recovery-runbook-automation/19.png)

## <a name="sample-scripts"></a>Mintaszkriptek
Amíg azt telefonon automatizálása egy gyakran használt feladat ebben az oktatóanyagban egy végpont tooan Azure virtuális gép hozzáadása, akkor megteheti más erőteljes automatizálás feladatot használ az Azure automation. A Microsoft és a hello Azure Automation közösségi biztosít a példa runbookok, amelyek segítségével elsajátíthatja a saját megoldások és segédprogram runbookok, amelyek nagyobb automatizálási feladatok építőelemeiként használhat. Indítsa el a használatuk hello gyűjteményből, és az Azure Site Recovery segítségével alkalmazások hatékony egyetlen kattintással a helyreállítási terv létrehozása.

## <a name="additional-resources"></a>További források
[Azure Automation szolgáltatásbeli áttekintése](http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure automatizálása – áttekintés")

[Azure Automation-parancsfájlok minta](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "Azure automatizálási parancsfájlokat minta")
