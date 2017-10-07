---
title: "aaaAzure DevTest Labs – gyakori kérdések |} Microsoft Docs"
description: "Válaszok toocommon Azure DevTest Labs kérdések"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: afe83109-b89f-4f18-bddd-b8b4a30f11b4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: tarcher
ms.openlocfilehash: 07d4c870eca21856750a472ed503de4a2734a438
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-devtest-labs-faq"></a>Azure DevTest Labs GYIK
Ebben a cikkben megválaszolunk néhány hello Azure DevTest Labs kapcsolatos általános kérdésekre.

**Általános**
## <a name="what-if-my-question-isnt-answered-here"></a>Mi történik, ha a fentiekben itt nem választ?
Ha a kérdését itt nem látható, ossza meg velünk, tudunk segíteni, hogy választ találjanak.

* Fel kérdést hello [disqus-beszélgetésben teheti szál](#comments) hello végén a kérdések és foglalkozó hello Azure Cache csoport és a Közösség más tagjaival kapcsolatos cikkben.
* egy szélesebb körű célközönség tooreach fel kérdést a hello [Azure DevTest Labs MSDN fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs), és foglalkozó hello Azure DevTest Labs csapat és hello Közösség más tagjai.
* a szolgáltatás kérése toomake küldje el a kérelmek és ötleteket toohello [Azure DevTest Labs User Voice](https://feedback.azure.com/forums/320373-azure-devtest-labs).

## <a name="why-should-i-use-azure-devtest-labs"></a>Miért érdemes használni Azure DevTest Labs szolgáltatásban?
Az Azure DevTest Labs szolgáltatásban a csapat időt és pénzt takaríthat meg. Fejlesztők saját segítségével számos különböző kiindulási környezetek létrehozása, és használja az összetevők tooquickly telepítheti és konfigurálhatja az alkalmazásokat. Az egyéni lemezképek és a formulák, virtuális gépek is menthetők, sablonok és egyszerűen másolható. Emellett labs kínálnak, amelyekkel a labor rendszergazdák tooreduce hulladék, és egy csapat környezetek kezelése konfigurálható házirendek. Ezek a házirendek automatikus leállítási, költség küszöbértéket, felhasználó és a maximális Virtuálisgép-méretek maximális gépek tartalmazza. Azure DevTest Labs részletesebb leírását, olvassa el a hello [áttekintése](devtest-lab-overview.md) vagy figyelési hello [bevezető videó](/documentation/videos/videos/what-is-azure-devtest-labs).

## <a name="what-does-worry-free-self-service-mean"></a>Mit jelent "aggódjon mentes, önkiszolgáló"?
"Aggódjon ingyenes önkiszolgáló" azt jelenti, hogy a fejlesztők és a tesztelést a saját környezetben szükség szerint, a rendszergazdáknak hello biztonsági annak ismerete, hogy Azure DevTest Labs csökkentheti hulladék és a költségek szabályozásához. A rendszergazdák adhat meg, melyik Virtuálisgép-méretek engedélyezett hello virtuális gépek maximális számát, és ha a virtuális gépek indítása és leállítása. Az Azure DevTest Labs is teszi, hogy könnyen toomonitor költségeket és a beállított riasztásokat toostay tudomást hogyan hello laborban erőforrásokat használnak.

## <a name="how-can-i-use-azure-devtest-labs"></a>Hogyan használható az Azure DevTest Labs szolgáltatásban?
Az Azure DevTest Labs akkor hasznos, bármikor szükséges fejlesztési vagy tesztelési környezetben, és azt szeretné, hogy tooreproduce őket gyorsan és/vagy kezelheti azokat a házirendeket mentése költség.

Az alábbiakban néhány forgatókönyv használó ügyfeleink Azure DevTest Labs:

* Egy helyen fejlesztési és tesztkörnyezetek kezelése, házirendek tooreduce költségek és az egyéni lemezképek tooshare használatával épít, hello csoport között.
* Egyéni lemezképek toosave hello lemez állapota hello fejlesztési műveletben használó alkalmazások fejlesztésével.
* A kapcsolat tooprogress hello költség nyomon követéséhez.
* Minőségi megbízhatósági tesztelési tömeges tesztkörnyezetek létrehozása.
* Az összetevők és a formulák tooeasily használatával konfigurálja, és Reprodukálja a különböző környezetek alkalmazás.
* Virtuális gépek kiosztása a hackathons (fejlesztési és tesztelési együttműködés), és majd könnyen megszüntetést őket hello esemény végén.

## <a name="how-am-i-billed-for-azure-devtest-labs"></a>Hogyan vagyok fizetni az Azure DevTest Labs szolgáltatásban?
Azure DevTest Labs szolgáltatásban egy olyan ingyenes szolgáltatás, ami azt jelenti, hogy labs létrehozásáról és konfigurálásáról a hello házirendek, sablonok és összetevők szabad. Csak fizetnie hello Azure-erőforrások, például a virtuális gépek, a storage-fiókok és a virtuális hálózatok a labs történik. Tesztlabor-erőforrások hello költségek további információkért olvassa el [Azure DevTest Labs árképzési](https://azure.microsoft.com/pricing/details/devtest-lab/).


**Biztonság**
## <a name="what-are-hello-different-security-levels-in-azure-devtest-labs"></a>Mik azok a különböző biztonsági szintek hello Azure DevTest Labs szolgáltatásban?
Biztonsági hozzáférést határozza meg [átruházásához hozzáférés-vezérlés (RBAC)](../active-directory/role-based-access-built-in-roles.md). toounderstand hogyan férhetnek hozzá a működését, hanem toounderstand hello különbségei engedélyt, a szerepkör és egy hatókör RBAC által definiált konfigurációjának kialakításához.

* **Engedély** -engedély olyan definiált hozzáférési tooa adott műveletet. Például egy engedély lehet írásvédett tooall virtuális gépek.
* **Szerepkör** -szerepkör csoportosítva, és hozzárendelt tooa felhasználó engedélyekkel. Például egy "előfizetés" tulajdonosnak előfizetésen belül tooall erőforrások eléréséhez.
* **Hatókör** -hatókör egy szint, az Azure-erőforrás hello hierarchián belül. A hatókör lehet például egy erőforráscsoport vagy egy-egy laboratóriumi vagy hello teljes előfizetéshez.

Az Azure DevTest Labs hello hatókörbe szerepkörök toodefine felhasználói engedélyek két típusa van: a tesztkörnyezet tulajdonosa és lab-felhasználó.

* **A tesztkörnyezet tulajdonosa** -tesztkörnyezet tulajdonosa hello tooany erőforrások eléréséhez hello laboron belüli. Ezért azokat is házirendek módosíthatók, olvasási és írási bármely virtuális gépek, hello virtuális hálózat módosítása, és így tovább.
* **Lab-felhasználó** - lab-felhasználó megtekintheti a tesztlabor összes erőforrás, például a virtuális gépek, a házirendek és a virtuális hálózatok, de nem módosíthatják a szabályzatokat vagy bármely más felhasználók által létrehozott virtuális gépek. Is lehetséges toocreate egyéni szerepkörök az Azure DevTest Labs szolgáltatásban, és áttekintheti, hogyan hello cikkben toodo [felhasználói engedélyek toospecific labor házirendek](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Mivel a hatókörök hierarchikus, amikor egy felhasználó jogosult bizonyos hatókörre, ezeket az engedélyeket minden alacsonyabb szintű hatókörből lefedett automatikusan kapnak. Például ha egy felhasználó az előfizetés tulajdonosa toohello szerepkör van hozzárendelve, majd rendelkeznek tooall erőforrások eléréséhez az előfizetést. Ilyen erőforrások többek között az összes virtuális gépet, az összes virtuális hálózatot és az összes labs. Egy előfizetés tulajdonosa így, automatikusan örökli a tesztkörnyezet tulajdonosa hello szerepe. Ellenkező hello azonban nem értéke true. A tesztkörnyezet tulajdonosa mint hello előfizetés szintje alacsonyabb hatókör hozzáférés tooa labor rendelkezik. A tesztkörnyezet tulajdonosa ezért nem képes toosee virtuális gépek vagy virtuális hálózatok, vagy minden olyan erőforrásnál, amely hello labor kívül esnek.

## <a name="how-do-i-create-a-role-tooallow-users-tooperform-a-specific-task"></a>Hogyan hozható létre egy szerepkör tooallow felhasználók tooperform egy adott feladat?
Egy átfogó cikk arról, hogyan toocreate egyéni szerepkörök és hozzárendelése engedélyek toothat szerepkör itt található. Íme egy példa a parancsfájl által létrehozott hello szerepkör "DevTest Labs speciális felhasználó", amelyet engedély toostart és hello tesztlabor virtuális gépeinek leállítása:

    $policyRoleDef = Get-AzureRmRoleDefinition "DevTest Labs User"
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*')
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "DevTest Labs Advance User"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action")
    $policyRoleDef = New-AzureRmRoleDefinition -Role $policyRoleDef  


**CI/CD-integráció és automatizálás**
## <a name="does-azure-devtest-labs-integrate-with-my-cicd-toolchain"></a>Azure DevTest Labs szolgáltatásban a CI/CD toolchain integrálása?
VSTS használ, hogy van-e egy [Azure DevTest Labs feladatok bővítmény](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) , amely lehetővé teszi, hogy Azure DevTest Labs-feldolgozási folyamat kiadási tooautomate. A bővítmény hello használatát többek között:

* Létrehozása és telepítése automatikusan a virtuális gép, és konfigurálni kell hello Azure fájl másolása vagy a PowerShell VSTS feladatainak legújabb buildjével.
* Automatikusan rögzítése tooreproduce programhiba vizsgálatot követően a virtuális gépek állapotának hello hello azonos virtuális gép további vizsgálatok esetén.
* Hello törlése hello hello végén VM felszabadíthatják a folyamat már nincs szükség.

hello következő blogbejegyzésekben útmutatást és információkat biztosítanak hello VSTS-kiterjesztés használatával:

* [Az Azure DevTest Labs szolgáltatásban – VSTS-bővítmény](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/)
* [Egy meglévő AzureDevTestLab a VSTS az új virtuális gép telepítése](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS)
* [A folyamatos központi telepítések tooAzureDevTestLabs VSTS Kiadáskezelés használatával](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs)

A többi CI/CD toolchains összes hello azt már korábban említettük VSTS feladatok bővítmény hasonlóképpen elérhető üzembe helyezhet hello keresztül elérhető forgatókönyvek [Azure Resource Manager-sablonok](https://aka.ms/dtlquickstarttemplate) használatával [ Az Azure PowerShell-parancsmagok](../azure-resource-manager/resource-group-template-deploy.md) és [.NET SDK-k](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/). Is [REST API-k a DevTest Labs](http://aka.ms/dtlrestapis) toointegrate a toolchain együtt.  


**Virtuális gépek**
## <a name="why-cant-i-see-certain-vms-in-hello-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs"></a>Miért nem látom, hogy egyes virtuális gépek hello Azure virtuális gépek panelen, amely látható Azure DevTest Labs belül?
A virtuális gép létrehozásakor a Azure DevTest Labs szolgáltatásban, az engedély van megadva tooaccess ezt a virtuális Gépet. Biztosan tudni tooview, mind a hello labs panel és hello **virtuális gépek** panelen. Hello DevTest Labs szerepet betöltő felhasználók látható hello laborban hello labor keresztül létrehozott összes virtuális gép **az összes virtuális gép** panelen. Azonban hello DevTest Labs szerepet betöltő felhasználók nem automatikusan rendelkeznek, mások hozott létre írásvédett tooVM erőforrásokat. Ezért, virtuális gépek nem jelennek meg hello **virtuális gépek** panelen.

## <a name="what-is-hello-difference-between-custom-images-and-formulas"></a>Mi az az egyéni lemezképek és a formulák hello különbségének?
Egyéni lemezkép a virtuális merevlemez (virtuális merevlemez), mivel képlet egy olyanra, amely további beállítások is mentheti, és Reprodukálja konfigurálható. Egyéni lemezkép előnyös, ha azt szeretné, tooquickly hozzon létre több környezetben hello lehet ugyanazon alapvető, nem módosítható lemezképét. Lehet, hogy jobb, ha azt szeretné, hogy a virtuális gép hello legújabb bits, egy virtuális hálózatot/alhálózatot vagy meghatározott méretet tooreproduce hello konfigurálása egy képletet. A részletesebb magyarázatáért lásd: hello cikk [az egyéni lemezképek és a DevTest Labs szolgáltatásban képletek összehasonlítása](devtest-lab-comparing-vm-base-image-types.md).

## <a name="how-do-i-create-multiple-vms-from-hello-same-template-at-once"></a>Hogyan hozható létre több virtuális gép a hello ugyanazt a sablont egyszerre?
Használhatja a hello [VSTS feladatok bővítmény](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) vagy [Azure Resource Manager-sablonok készítése](devtest-lab-add-vm.md#save-azure-resource-manager-template) a virtuális gépek létrehozásakor és [hello Azure Resource Manager-sablon a Windows PowerShell telepítése ](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab"></a>Milyen a meglévő Azure virtuális gépek áthelyezi a saját Azure DevTest Labs labor?
A meglévő virtuális gépek tooAzure DevTest Labs adjon kövesse az alábbi toocopy hello lépéseket:

1. Másolás hello VHD-fájlt a meglévő virtuális gép használatát az [Windows PowerShell-parancsfájl](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1)
2. [Hozzon létre egyéni lemezkép hello](devtest-lab-create-template.md) a Azure DevTest Labs labor belül.
3. Hello, amikor az egyéni lemezképet a virtuális gép létrehozása

## <a name="can-i-attach-multiple-disks-toomy-vms"></a>Csatolható több lemezek toomy virtuális gépek?
Több lemez tooVMs csatolása esetén támogatott.  

## <a name="if-i-want-toouse-a-windows-os-image-for-my-testing-do-i-have-toopurchase-an-msdn-subscription"></a>Ha a Windows operációs rendszer lemezképének toouse szeretnék saját tesztelési, kell toopurchase az MSDN előfizetői?
Ha a fejlesztési vagy tesztelési az Azure-ban kell toouse Windows ügyfél operációs rendszer lemezképeit (Windows 7 vagy újabb), majd Igen, először:

- [Az MSDN-előfizetés vásárlása](https://www.visualstudio.com/products/how-to-buy-vs).
- Ha egy nagyvállalati szerződés, hozzon létre egy Azure-előfizetés hello [vállalati fejlesztési és tesztelési célú ajánlat](https://azure.microsoft.com/en-us/offers/ms-azr-0148p).

További információ a hello minden MSDN ajánlat Azure-krediteket: [havi Azure-kredit a Visual Studio-előfizetők](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/).

## <a name="how-do-i-automate-hello-process-of-uploading-vhd-files-toocreate-custom-images"></a>Hogyan automatizálni a VHD-fájlok toocreate egyéni lemezképek feltöltése hello folyamat?
Két lehetőség áll rendelkezésre:

* [Az Azure AzCopy](../storage/common/storage-use-azcopy.md#blob-upload) használt toocopy és töltse fel a VHD fájlokat toohello tárfiók hello labor társított.
* [A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy különálló alkalmazás, a Windows, OSX és Linux futtató.   

toofind hello céltárfiókot a labor társított kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Válassza ki **erőforráscsoportok** hello bal oldali panelen.
3. Keresse meg és jelölje ki a labor társított hello erőforráscsoportot.
4. A hello **áttekintése** panelen válasszon ki egy hello tárfiókok.
5. Válassza ki **Blobok**.
6. Keresse meg a hello listában feltöltések. Ha még nem létezik ilyen, vissza tooStep #4, és próbálja meg egy másik tárfiókhoz.
7. Használjon hello **URL-cím** az AzCopy parancs a célként.

## <a name="how-can-i-automate-hello-process-of-deleting-all-hello-vms-in-my-lab"></a>Hogyan automatizálható a laborban összes hello virtuális gépek törlésével hello folyamat?
Ezenkívül toodeleting virtuális gépek a hello Azure-portálon a labor, törölheti hello virtuális gépeinek a tesztkörnyezetben egy PowerShell-parancsfájl használatával. A következő példában hello hello paraméterértékek hello alatt módosítsa **értékek toochange** megjegyzés. Hello le `subscriptionId`, `labResourceGroup`, és `labName` hello labor panel az Azure-portálon hello értékeit.

    # Delete all hello VMs in a lab

    # Values toochange
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Login tooyour Azure account
    Login-AzureRmAccount

    # Select hello Azure subscription that contains hello lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Get hello lab that contains hello VMs toodelete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)

    # Get hello VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object {
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}

    # Delete hello VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }

**Az összetevők**
## <a name="what-are-artifacts"></a>Mik azok az összetevők?
Összetevők olyan testreszabható elemei, amelyek a legfrissebb bit vagy a fejlesztői eszközök, a virtuális gépek használt toodeploy lehet. Csatolt tooyour VM néhány egyszerű kattintással létrehozása során, és hello virtuális gép üzembe helyezése után hello összetevők telepítése és konfigurálása a virtuális Gépet. A különböző már meglévő összetevők van a [nyilvános GitHub-tárházban](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts), azonban úgy is könnyen is [hozhatnak létre a saját összetevők](devtest-lab-artifact-author.md).


**Tesztlabor-konfiguráció**
## <a name="how-do-i-create-a-lab-from-an-azure-resource-manager-template"></a>Hogyan hozható létre egy, amikor az Azure Resource Manager sablon alapján?
Adtunk egy [labor Azure Resource Manager-sablonok a GitHub-tárházban](https://aka.ms/dtlquickstarttemplate) , hogy telepítheti-, vagy módosítsa a labs toocreate egyéni sablonokat. Ezek a sablonok mindegyikén toodeploy hello labor, kattintson a hivatkozásra-alatt a saját Azure-előfizetésre, vagy testre szabhatja a hello sablon és [a PowerShell vagy az Azure parancssori felület telepítése](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Miért létrejönnek a virtuális gépek különböző erőforráscsoportokra tetszőleges nevű? Nevezze át vagy módosítsa ezeket az erőforráscsoportokat?
Erőforráscsoportok ahhoz, hogy Azure DevTest Labs toomanage hello felhasználói engedélyekkel és hozzáférési toovirtual gépek ily módon jönnek létre. Hello VM tooanother erőforráscsoport áthelyezheti a kívánt néven, amíg ez nem ajánlott. Dolgozunk a felhasználói élmény tooallow nagyobb rugalmasságot javítása.   

## <a name="how-many-labs-can-i-create-under-hello-same-subscription"></a>Hány labs hozható létre a hello ugyanazt az előfizetést?
Nincs adott korlát előfizetésenként létrehozható labs hello számára. Azonban a használt hello erőforrások korlátozva előfizetésenként. Hello olvashat [korlátozásai és a kvóták kivetett Azure-előfizetések](../azure-subscription-service-limits.md) és [hogyan tooincrease működés felső korlátjának](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-many-vms-can-i-create-per-lab"></a>Hány virtuális gépeket hozhat létre száma tesztkörnyezetenként?
A virtuális gépek száma tesztkörnyezetenként létrehozható hello száma adott korlátozva van. Azonban használt hello erőforrások korlátozódnak előfizetésenként (pl. virtuális gép mag, nyilvános IP-címek, stb.). Hello olvashat [korlátozásai és a kvóták kivetett Azure-előfizetések](../azure-subscription-service-limits.md) és [hogyan tooincrease működés felső korlátjának](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-do-i-share-a-direct-link-toomy-lab"></a>A közvetlen hivatkozás toomy labor megosztása?
a közvetlen kapcsolat tooshare tooyour lab-felhasználó, hajthat végre a következő eljárás hello:

1. Keresse meg a labor toohello hello Azure-portálon.
2. Hello labor URL-Címének másolása a böngészőből, és ossza meg a labor felhasználóival.

> [!NOTE]
> Ha a labor felhasználók a külső felhasználók egy [Microsoft-fiók](#what-is-a-microsoft-account) és tooyour vállalati Active Directoryhoz nem tartoznak, amikor máshová toohello megadott hivatkozás hiba jelenhet. Ha hibaüzenetet kapnak, utasítsa őket hello jobb felső sarkában nevük hello Azure-portálon, és válassza hello directory hello labor esetén a hello tooclick **Directory** hello menü részét.
>
>

## <a name="what-is-a-microsoft-account"></a>Mi az Microsoft-fiókkal?
Microsoft-fiókkal szinte minden Microsoft-eszközöket és a szolgáltatások használatára. Egy e-mail címet és egy jelszót a tooSkype, Outlook.com, OneDrive, Windows Phone és Xbox LIVE – toosign használja, és azt jelenti, hogy a fájlokat, fényképek, névjegyek és beállítások követheti, hogy tooany eszköz.

> [!NOTE]
> Microsoft-fiók "Windows Live ID" nevű toobe használt.
>
>


**hibaelhárítással**
## <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Az összetevő virtuális gép létrehozása sikertelen volt. Hogyan hibaelhárítása azt?
Tekintse meg a túl[hogyan toodiagnose összetevő hibáinak DevTest Labs](devtest-lab-troubleshoot-artifact-failure.md) toolearn hogyan tooobtain naplózza a sikertelen összetevő kapcsolatban.

## <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Miért nem a meglévő virtuális hálózat mentése megfelelően?
Több lehetősége, hogy a virtuális hálózati pontokat tartalmazza. Ha igen, próbálja meg eltávolítani hello pontot és kötőjelet lecserélése, és ismételje meg a Mentés újra hello virtuális hálózat.

## <a name="why-do-i-get-a-parent-resource-not-found-error-when-provisioning-a-vm-from-powershell"></a>Miért kapok "Szülő erőforrás nem található" hibaüzenet, ha egy virtuális Gépet a PowerShell?
Amikor egy erőforrást egy szülő tooanother erőforrás, hello szülő erőforrás hello gyermek-erőforrás létrehozása előtt léteznie kell. Ha nem létezik, akkor kap egy **ParentResourceNotFound** hiba. Ha nincs megadva a függőség hello szülő erőforrás, hello gyermek erőforrás hello szülő előtt telepíthető.

Virtuális gépek gyermekerőforrásait egy erőforráscsoportban, amikor a rendszer. Azure Resource Manager sablonok toodeploy keresztül PowerShell használata esetén az erőforráscsoport neve hello hello PowerShell-parancsfájl a megadott hello erőforráscsoport-név hello labor kell lennie. További információkért lásd: [az Azure-telepítés gyakori hibáinak az elhárítását ](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-common-deployment-errors#parentresourcenotfound).

## <a name="where-can-i-find-more-error-information-if-a-vm-deployment-fails"></a>Hol találok további hibainformációk, ha egy virtuális gép telepítése sikertelen?
Virtuális gép telepítési hibák hello tevékenység naplózva lesznek rögzítve. Található labor virtuális gépeken tevékenységi naplóit keresztül hello **naplók** vagy **virtuálisgép-diagnosztika** hello erőforrás menü hello tesztlabor virtuális gép paneljén (hello panelt jeleníti meg, miután kiválasztotta a virtuális gép hello a**a virtuális gépek** lista).

Hello központi telepítési hiba néha hello Virtuálisgép-telepítéshez elindulása – például amikor hello előfizetés hello VM létrehozott erőforrás letelte előtt következik be. Ebben az esetben rögzített hello hiba legutolsó részletes adatai hello labor szint **tevékenységi naplóit** , amely lehet hello hello alján található **konfigurációs és házirendek** beállításait. Tevékenység használatáról további információkat naplózza az Azure-ban, a következő témakörben: [tevékenység naplózza tooaudit műveleteket az egyes erőforrások megtekintése](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-audit).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]
