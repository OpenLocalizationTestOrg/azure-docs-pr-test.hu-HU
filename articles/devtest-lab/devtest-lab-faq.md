---
title: "Az Azure DevTest Labs – gyakori kérdések |} Microsoft Docs"
description: "Közös Azure DevTest Labs kérdésekre adott válaszok"
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
ms.openlocfilehash: 8ee4a0fd714027cdc77247ec8dc259fe62442564
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-devtest-labs-faq"></a>Azure DevTest Labs GYIK
Ebben a cikkben megválaszolunk Azure DevTest Labs kapcsolatos általános kérdésekre.

**Általános**
## <a name="what-if-my-question-isnt-answered-here"></a>Mi történik, ha a fentiekben itt nem választ?
Ha a kérdését itt nem látható, ossza meg velünk, tudunk segíteni, hogy választ találjanak.

* A elküldésekor a [disqus-beszélgetésben teheti szál](#comments) végén a kérdések és az Azure Cache csapat és a Közösség más tagjaival kapcsolatos cikkben vállalhat.
* Egy szélesebb körű célközönség eléréséhez fel kérdést az [Azure DevTest Labs MSDN fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs), és az Azure DevTest Labs csoportját, és a Közösség más tagjai bevonásához.
* Ahhoz, hogy a szolgáltatás kérése, küldje el a kérelmek és ötleteket a a [Azure DevTest Labs User Voice](https://feedback.azure.com/forums/320373-azure-devtest-labs).

## <a name="why-should-i-use-azure-devtest-labs"></a>Miért érdemes használni Azure DevTest Labs szolgáltatásban?
Az Azure DevTest Labs szolgáltatásban a csapat időt és pénzt takaríthat meg. A fejlesztők saját segítségével számos különböző kiindulási környezetek létrehozása, és az összetevők segítségével gyorsan telepítheti és konfigurálhatja az alkalmazásokat. Az egyéni lemezképek és a formulák, virtuális gépek is menthetők, sablonok és egyszerűen másolható. Ezenkívül labs kínálnak, amelyek lehetővé teszik a labor rendszergazdáknak, hogy csökkentse a pazarlás és egy csapat környezetek kezelése konfigurálható házirendek. Ezek a házirendek automatikus leállítási, költség küszöbértéket, felhasználó és a maximális Virtuálisgép-méretek maximális gépek tartalmazza. Egy Azure DevTest Labs részletesebb magyarázatra, olvassa el a [áttekintése](devtest-lab-overview.md) vagy tekintse meg a [bevezető videó](/documentation/videos/videos/what-is-azure-devtest-labs).

## <a name="what-does-worry-free-self-service-mean"></a>Mit jelent "aggódjon mentes, önkiszolgáló"?
"Aggódjon ingyenes önkiszolgáló" azt jelenti, hogy a fejlesztők és a tesztelést a saját környezetben szükség szerint, a rendszergazdáknak, hogy tudnák, hogy Azure DevTest Labs csökkentheti a biztonsági, hulladék és a költségek szabályozásához. A rendszergazdák adhat meg, melyik Virtuálisgép-méretek engedélyezett virtuális gépek maximális számát, és ha a virtuális gépek indítása és leállítása. Az Azure DevTest Labs szolgáltatásban is egyszerűen figyelheti a költségek, és állítson be riasztásokat a tudomást milyen erőforrásokat a laborban használnak.

## <a name="how-can-i-use-azure-devtest-labs"></a>Hogyan használható az Azure DevTest Labs szolgáltatásban?
Az Azure DevTest Labs akkor hasznos, bármikor szükséges fejlesztési vagy tesztelési környezetben, és gyorsan reprodukálásához szükséges és/vagy kezelheti azokat a házirendeket mentése költség szeretné.

Az alábbiakban néhány forgatókönyv használó ügyfeleink Azure DevTest Labs:

* Egy helyen fejlesztési és tesztkörnyezetek kezelése, a csapat közötti költségek és az egyéni lemezképek megosztásához csökkentése érdekében a házirendek használatával épít.
* Az egyéni lemezképek használatával menti a lemez állapota a fejlesztési műveletben alkalmazások fejlesztése.
* Nyomon követi a költségeket viszonyítva folyamatban van.
* Minőségi megbízhatósági tesztelési tömeges tesztkörnyezetek létrehozása.
* Összetevők és a formulák segítségével könnyen konfigurálása és az alkalmazás különböző környezetekben Reprodukálja.
* Virtuális gépek kiosztása a hackathons (fejlesztési és tesztelési együttműködés), és majd könnyen megszüntetést azokat az esemény végén.

## <a name="how-am-i-billed-for-azure-devtest-labs"></a>Hogyan vagyok fizetni az Azure DevTest Labs szolgáltatásban?
Azure DevTest Labs szolgáltatásban egy olyan ingyenes szolgáltatás, ami azt jelenti, hogy labs létrehozásához és konfigurálásához, a házirendek, sablonok és összetevők szabad. Csak az Azure-erőforrások, például a virtuális gépek, a storage-fiókok és a virtuális hálózatok a labs belül használt fizetnie. Tesztlabor-erőforrások a költségek további információkért olvassa el [Azure DevTest Labs árképzési](https://azure.microsoft.com/pricing/details/devtest-lab/).


**Biztonság**
## <a name="what-are-the-different-security-levels-in-azure-devtest-labs"></a>Mik a különböző védelmi szintek a Azure DevTest Labs szolgáltatásban?
Biztonsági hozzáférést határozza meg [átruházásához hozzáférés-vezérlés (RBAC)](../active-directory/role-based-access-built-in-roles.md). Hozzáférés működésének megismerése segít engedélyt, a szerepkör és a Szerepalapú által meghatározott hatókör közötti különbségek megismeréséhez.

* **Engedély** -engedély egy bizonyos művelet meghatározott elérésére. Például egy engedély olvasási-hozzáférés a virtuális gépek lehet.
* **Szerepkör** -szerepkör csoportosítva, és a felhasználóhoz rendelt engedélyek egy készletét. Például egy "előfizetés tulajdonosa" hozzáfér egy előfizetésen belüli összes erőforrást.
* **Hatókör** -hatókör egy szint, az Azure-erőforrás a hierarchiában. A hatókör lehet például egy erőforráscsoport vagy egy laboratóriumi vagy az egész előfizetésre.

Azure DevTest Labs keretén belül a felhasználói engedélyek megadásához szerepkörök két típusa van: a tesztkörnyezet tulajdonosa és lab-felhasználó.

* **A tesztkörnyezet tulajdonosa** – A labor a tulajdonosnak a hozzáférni semmilyen erőforráshoz belül. Ezért azokat is házirendek módosíthatók, olvasási és írási bármely virtuális gépek, módosítsa a virtuális hálózatot, és így tovább.
* **Lab-felhasználó** - lab-felhasználó megtekintheti a tesztlabor összes erőforrás, például a virtuális gépek, a házirendek és a virtuális hálózatok, de nem módosíthatják a szabályzatokat vagy bármely más felhasználók által létrehozott virtuális gépek. Egyéni szerepkörök létrehozása a Azure DevTest Labs szolgáltatásban lehetőség arra is, és megismerheti a cikk az [felhasználói engedélyeket adott labor házirendek](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Mivel a hatókörök hierarchikus, amikor egy felhasználó jogosult bizonyos hatókörre, ezeket az engedélyeket minden alacsonyabb szintű hatókörből lefedett automatikusan kapnak. Például ha a felhasználó a szerepkört az előfizetés tulajdonosa van rendelve, majd hozzáférhetnek összes erőforrást egy előfizetésben. Ilyen erőforrások többek között az összes virtuális gépet, az összes virtuális hálózatot és az összes labs. Egy előfizetés tulajdonosa így, automatikusan örökli a tesztkörnyezet tulajdonosa szerepe. Azonban a ennek fordítottja nem igaz. A tesztkörnyezet tulajdonosa hozzáfér egy tesztkörnyezetet, amelyhez a hatókör alacsonyabb, mint az előfizetés szintjéről. A tesztkörnyezet tulajdonosa ezért nem láthatók a virtuális gépek vagy virtuális hálózatok, vagy minden olyan erőforrásnál, amely a labor kívül esnek.

## <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Hogyan hozható létre a felhasználók egy adott feladat végrehajtásához szerepkör?
Hozzon létre egyéni szerepköröket és engedélyeket ehhez a szerepkörhöz egy átfogó cikkben található. Íme egy példa egy parancsfájlt, amely a szerepkör "DevTest Labs speciális felhasználó", amelyiknek engedélye van indítása és leállítása a tesztlabor virtuális gépeinek hoz létre:

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
VSTS használ, hogy van-e egy [Azure DevTest Labs feladatok bővítmény](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) , amely lehetővé teszi, hogy automatizálja a kiadási folyamat a Azure DevTest Labs szolgáltatásban. A bővítmény használatát, többek között:

* Létrehozása és telepítése automatikusan a virtuális gép, és konfigurálni kell az Azure a fájl másolása vagy a PowerShell VSTS feladatainak legújabb buildjével.
* Automatikusan melynek az állapotát rögzíti a virtuális gépek reprodukálásához szükséges további vizsgálatok esetén az azonos virtuális gépen programhiba tesztelés után.
* A kiadási folyamat végén a virtuális gép törlése, ha már nincs szükség.

A következő blogbejegyzésekben adja meg az útmutató és a VSTS-bővítmény használatával kapcsolatos információk:

* [Az Azure DevTest Labs szolgáltatásban – VSTS-bővítmény](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/)
* [Egy meglévő AzureDevTestLab a VSTS az új virtuális gép telepítése](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS)
* [Folyamatos központi telepítésére AzureDevTestLabs VSTS Kiadáskezelés használatával](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs)

A többi CI/CD toolchains a korábban említett forgatókönyvek az elérhető feladatok VSTS bővítményével hasonló módon érheti el üzembe helyezhet [Azure Resource Manager-sablonok](https://aka.ms/dtlquickstarttemplate) használatával [Azure PowerShell-parancsmagok](../azure-resource-manager/resource-group-template-deploy.md) és [.NET SDK-k](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/). Is [REST API-k a DevTest Labs](http://aka.ms/dtlrestapis) a toolchain integrálható.  


**Virtual Machines**
## <a name="why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs"></a>Miért nem látom, hogy az Azure virtuális gépek panel, amelyen belül Azure DevTest Labs látható az egyes virtuális gépek?
Egy virtuális gép létrehozásakor a Azure DevTest Labs szolgáltatásban, engedélyt kap arra, hogy ezt a virtuális Gépet eléréséhez. Tekintse meg mindkét a labs panel válik, és a **virtuális gépek** panelen. A DevTest Labs szolgáltatásban a szerepkörben levő felhasználók láthatja az összes virtuális gépet, a laborban keresztül a labor létrehozása **az összes virtuális gép** panelen. Azonban a DevTest Labs szolgáltatásban a szerepkörben levő felhasználók nem automatikusan rendelkeznek, mások hozott létre Virtuálisgép-erőforrások elérésére. Ezért, virtuális gépek nem jelennek meg a **virtuális gépek** panelen.

## <a name="what-is-the-difference-between-custom-images-and-formulas"></a>Mi az a különbség az egyéni lemezképek és a formulák között?
Egyéni lemezkép a virtuális merevlemez (virtuális merevlemez), mivel képlet egy olyanra, amely további beállítások is mentheti, és Reprodukálja konfigurálható. Lehet, hogy hatékonyabb, ha azt szeretné, létrehozhat több környezetben ugyanazon alapvető, nem módosítható lemezképét egy egyéni lemezképet. Lehet, hogy egy képletet jobb, ha azt szeretné, hogy a virtuális Gépet a legújabb bits, egy virtuális hálózatot/alhálózatot vagy meghatározott méretet konfigurációjának reprodukálásához szükséges. A részletesebb magyarázatáért lásd: a cikk [az egyéni lemezképek és a DevTest Labs szolgáltatásban képletek összehasonlítása](devtest-lab-comparing-vm-base-image-types.md).

## <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Hogyan hozható létre több virtuális gép ugyanazon sablonból egyszerre?
Használhatja a [VSTS feladatok bővítmény](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) vagy [Azure Resource Manager-sablonok készítése](devtest-lab-add-vm.md#save-azure-resource-manager-template) közben a virtuális gép létrehozása és [a Windows PowerShellazAzureResourceManagersablonüzembehelyezése](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab"></a>Milyen a meglévő Azure virtuális gépek áthelyezi a saját Azure DevTest Labs labor?
Kérjük, kövesse az alábbi lépéseket a meglévő virtuális gépek másolása Azure DevTest Labs szolgáltatásban:

1. Másolja a VHD-fájlt a meglévő virtuális gép használatát az [Windows PowerShell-parancsfájl](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1)
2. [Az egyéni lemezképet](devtest-lab-create-template.md) a Azure DevTest Labs labor belül.
3. A laborkörnyezetben az egyéni lemezképet a virtuális gép létrehozása

## <a name="can-i-attach-multiple-disks-to-my-vms"></a>Hozzácsatlakoztathatok több lemezt a virtuális gépek?
Több lemez csatolása a virtuális gépek esetén támogatott.  

## <a name="if-i-want-to-use-a-windows-os-image-for-my-testing-do-i-have-to-purchase-an-msdn-subscription"></a>A Windows operációs rendszer lemezképét a a teszteléshez használni kívánt, ha rendelkezik az MSDN előfizetői?
Ha a Windows ügyfél operációs rendszer lemezképeit (Windows 7 vagy újabb) használó a fejlesztés és tesztelés az Azure-ban van szüksége, majd Igen, először:

- [Az MSDN-előfizetés vásárlása](https://www.visualstudio.com/products/how-to-buy-vs).
- Ha egy nagyvállalati szerződés, az Azure-előfizetés létrehozása a [vállalati fejlesztési és tesztelési célú ajánlat](https://azure.microsoft.com/en-us/offers/ms-azr-0148p).

Az Azure-krediteket az egyes MSDN ajánlat kapcsolatos további információkért lásd: [havi Azure-kredit a Visual Studio-előfizetők](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/).

## <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Hogyan automatizálhatók az egyéni lemezképek létrehozásához VHD-fájlok feltöltése a folyamatot?
Két lehetőség áll rendelkezésre:

* [Az Azure AzCopy](../storage/common/storage-use-azcopy.md#blob-upload) másolja vagy VHD-fájlok feltöltése a tárfiókba, a labor társított is használható.
* [A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy különálló alkalmazás, a Windows, OSX és Linux futtató.   

A cél tárfiókkal a labor társított megkereséséhez kövesse az alábbi lépéseket:

1. Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Válassza ki **erőforráscsoportok** a bal oldali panelen.
3. Keresse meg és jelölje ki a labor tartozó erőforráscsoport.
4. Az a **áttekintése** panelen, a storage-fiókok közül.
5. Válassza ki **Blobok**.
6. Keresse meg a listában feltöltések. Ha még nem létezik ilyen, térjen vissza #4. lépés, és próbálja meg egy másik tárfiókhoz.
7. Használja a **URL-cím** az AzCopy parancs a célként.

## <a name="how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Hogyan automatizálható a tesztlabor összes virtuális gép törlésének?
Mellett a tesztkörnyezet az Azure-portálon való törlésekor a virtuális gépeket, törölheti a virtuális gépeket a tesztkörnyezetben egy PowerShell-parancsfájl használatával. A paraméterértékek módosítsa a következő példában a **értékek módosítása** megjegyzés. Kérheti le a `subscriptionId`, `labResourceGroup`, és `labName` értékeit a labor panel az Azure portálon.

    # Delete all the VMs in a lab

    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Login to your Azure account
    Login-AzureRmAccount

    # Select the Azure subscription that contains the lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Get the lab that contains the VMs to delete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)

    # Get the VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object {
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}

    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }

**Az összetevők**
## <a name="what-are-artifacts"></a>Mik azok az összetevők?
Az összetevők olyan testreszabható elemei, amelyek segítségével telepítheti a legújabb bit vagy a fejlesztői eszközök, a virtuális gépek. A virtuális Géphez kapcsolódó néhány egyszerű kattintással létrehozása során, és után a virtuális gép ki van építve, az összetevők telepítése és konfigurálása a virtuális Gépet. A különböző már meglévő összetevők van a [nyilvános GitHub-tárházban](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts), azonban úgy is könnyen is [hozhatnak létre a saját összetevők](devtest-lab-artifact-author.md).


**Tesztlabor-konfiguráció**
## <a name="how-do-i-create-a-lab-from-an-azure-resource-manager-template"></a>Hogyan hozható létre egy, amikor az Azure Resource Manager sablon alapján?
Adtunk egy [labor Azure Resource Manager-sablonok a GitHub-tárházban](https://aka.ms/dtlquickstarttemplate) , hogy telepítheti-, vagy módosítsa a tesztkörnyezetekhez egyéni sablonok létrehozása. Ezek a sablonok mindegyike rendelkezik, a hivatkozást, amellyel a labor, mint a telepítendő kattinthat-alatt a saját Azure-előfizetésre, vagy testre szabhatja a sablon és [a PowerShell vagy az Azure parancssori felület telepítése](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Miért létrejönnek a virtuális gépek különböző erőforráscsoportokra tetszőleges nevű? Nevezze át vagy módosítsa ezeket az erőforráscsoportokat?
Erőforráscsoportok ahhoz, hogy a felhasználói engedélyek és a virtuális gépek való hozzáférés kezelése Azure DevTest Labs ily módon jönnek létre. A kívánt nevű a virtuális Gépet áthelyezheti egy másik erőforráscsoportban, amíg ez nem ajánlott. Ez a felület további rugalmas javítása dolgozunk.   

## <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Hány labs hozhat létre a ugyanazt az előfizetést?
Nincs megadott korlát előfizetésenként létrehozható labs száma. Azonban a használt erőforrások korlátozva előfizetésenként. Tekintse át a a [korlátozásai és a kvóták kivetett Azure-előfizetések](../azure-subscription-service-limits.md) és [működés felső korlátjának növelésére](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-many-vms-can-i-create-per-lab"></a>Hány virtuális gépeket hozhat létre száma tesztkörnyezetenként?
A virtuális gépek száma tesztkörnyezetenként létrehozható számát az adott korlátozva van. Azonban a használt erőforrások korlátozódnak előfizetésenként (pl. virtuális gép mag, nyilvános IP-címek, stb.). Tekintse át a a [korlátozásai és a kvóták kivetett Azure-előfizetések](../azure-subscription-service-limits.md) és [működés felső korlátjának növelésére](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Hogyan megosztani az egy közvetlen hivatkozást a laborkörnyezetben?
A labor felhasználók egy közvetlen hivatkozást megosztásához hajthatja végre az alábbi eljárást:

1. Tallózással keresse meg a labor az Azure portálon.
2. A tesztkörnyezet URL-Címének másolása a böngészőből, és ossza meg a labor felhasználóival.

> [!NOTE]
> Ha a labor felhasználók a külső felhasználók egy [Microsoft-fiók](#what-is-a-microsoft-account) és nem tartoznak a vállalat Active directory, az alábbi hivatkozást megnyitásakor hiba jelenhet. Ha hibaüzenetet kapnak, utasítsa őket, kattintson a nevére, az Azure-portál jobb felső sarkában, és válassza ki a könyvtárat, ha a labor létezik-e a a **Directory** a menü részét.
>
>

## <a name="what-is-a-microsoft-account"></a>Mi az Microsoft-fiókkal?
Microsoft-fiókkal szinte minden Microsoft-eszközöket és a szolgáltatások használatára. Egy e-mail címet és jelszót, amely jelentkezhet Skype, Outlook.com, OneDrive, Windows Phone-és Xbox LIVE-, és azt jelenti, hogy a fájlokat, fényképek, névjegyek, és beállítások követve, bármilyen eszközről.

> [!NOTE]
> Microsoft-fiókot használ egykori "Windows Live ID".
>
>


**hibaelhárítással**
## <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Az összetevő virtuális gép létrehozása sikertelen volt. Hogyan hibaelhárítása azt?
Tekintse meg [a DevTest Labs szolgáltatásban összetevő hibák diagnosztizálása](devtest-lab-troubleshoot-artifact-failure.md) további naplók a sikertelen összetevő vonatkozó beszerzése.

## <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Miért nem a meglévő virtuális hálózat mentése megfelelően?
Több lehetősége, hogy a virtuális hálózati pontokat tartalmazza. Ha igen, próbálkozzon az időszakok eltávolítása vagy lecserélése kötőjeleket, és próbálja újból menteni a virtuális hálózat.

## <a name="why-do-i-get-a-parent-resource-not-found-error-when-provisioning-a-vm-from-powershell"></a>Miért kapok "Szülő erőforrás nem található" hibaüzenet, ha egy virtuális Gépet a PowerShell?
Amikor egy erőforrást egy másik erőforrásra való szülője, a szülő erőforrás léteznie kell a gyermek-erőforrás létrehozása előtt. Ha nem létezik, akkor kap egy **ParentResourceNotFound** hiba. Ha a szülő erőforrás függősége nincs megadva, a gyermek-erőforrás előtt a szülő telepíthető.

Virtuális gépek gyermekerőforrásait egy erőforráscsoportban, amikor a rendszer. Azure Resource Manager-sablonok használatával történő telepítése, a PowerShell segítségével, a PowerShell-parancsfájl a megadott erőforráscsoport-név – a labor az erőforráscsoport nevének kell lennie. További információkért lásd: [az Azure-telepítés gyakori hibáinak az elhárítását ](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-common-deployment-errors#parentresourcenotfound).

## <a name="where-can-i-find-more-error-information-if-a-vm-deployment-fails"></a>Hol találok további hibainformációk, ha egy virtuális gép telepítése sikertelen?
Virtuális gép központi telepítési hibái a tevékenység naplózva lesznek rögzítve. Labor virtuális gépeken tevékenységi naplóit keresztül is megtalálhatja a **naplók** vagy **virtuálisgép-diagnosztika** a labor virtuális gép paneljén erőforrás menüjének (a panelt jeleníti meg, miután kiválasztotta a virtuális gép **a virtuális gépek** lista).

Egyes esetekben a központi telepítési hiba akkor fordul elő, a virtuális gép telepítési kezdete - például ha az előfizetés létre a VM erőforrás letelte előtt. Ebben az esetben a hiba részletes adatait a labor szinten rofilok rögzítésének **tevékenységi naplóit** alján található, amely lehet a **konfigurációs és házirendek** beállításait. Tevékenység használatáról további információkat naplózza az Azure-ban, a következő témakörben: [tevékenységi naplóit rendszervizsgálati műveleteket az egyes erőforrások megtekintése](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-audit).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]
