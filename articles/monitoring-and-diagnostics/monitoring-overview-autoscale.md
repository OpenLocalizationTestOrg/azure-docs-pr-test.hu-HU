---
title: "az automatikus skálázási a Microsoft Azure virtuális gépek, a Cloud Services és a Web Apps aaaOverview |} Microsoft Docs"
description: "A Microsoft Azure-ban automatikus skálázás áttekintése. Érvényes tooVirtual gépek, a Cloud Services és a Web Apps."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 74bf03be-e658-4239-a214-c12424b53e4c
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2016
ms.author: robb
ms.openlocfilehash: ef68b722ea22c4149a7d7a89c5855ba761db9247
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-autoscale-in-microsoft-azure-virtual-machines-cloud-services-and-web-apps"></a>A Microsoft Azure virtuális gépek, a Cloud Services és a Web Apps automatikus skálázás áttekintése
A cikkből megtudhatja, milyen Microsoft Azure automatikus skálázás, annak előnyeit, és hogyan tooget használja azt.  

Az Azure a figyelő automatikus skálázás csak túl vonatkozik[virtuálisgép-méretezési csoportok](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Felhőszolgáltatások](https://azure.microsoft.com/services/cloud-services/), és [App Service - webalkalmazások](https://azure.microsoft.com/services/app-service/web/).

> [!NOTE]
> Azure automatikus skálázás két módszer van. Automatikus skálázás régebbi verziója tooVirtual gépek (rendelkezésre állási készletek) vonatkozik. Ez a szolgáltatás korlátozott mértékben támogatja, és ajánlott áttelepítése toovirtual virtulálisgép-skálázási készletekben gyorsabb és megbízhatóbb automatikus skálázás támogatásához. Hogyan toouse hello régebbi technológia ebben a cikkben szereplő hivatkozásra kattintva.  
>
>

## <a name="what-is-autoscale"></a>Mi az az automatikus skálázás?
Automatikus skálázás toohave hello jobb erőforrások mennyisége toohandle hello terhelés fut az alkalmazás lehetővé teszi. Ez lehetővé teszi tooadd erőforrások a toohandle növekedése betölteni, és is költségtakarékosabb munkavégzésben erőforrásokat, amelyek üresjárati ül eltávolításával. Adja meg a példányok toorun minimális és maximális számát, és adja hozzá vagy távolítsa el a virtuális gépek meghatározott szabályok alapján automatikusan. A minimális elérhetővé válnak meg arról, hogy az alkalmazás mindig fut még nincs terhelés alatt. A legfeljebb korlátozza a teljes lehetséges óránkénti költsége. Automatikus méretezése hoz létre szabályokkal e két érték között.

 ![Automatikus skálázás ismertetése. Hozzá és távolíthat el a virtuális gépek](./media/monitoring-overview-autoscale/AutoscaleConcept.png)

Ha a szabály feltételek teljesülnek, egy vagy több automatikus skálázási művelet aktiválódnak. Hozzáadása és eltávolítása a virtuális gépek vagy egyéb műveleteket hajthat végre. hello következő fogalmi diagramja jeleníti meg ezt a folyamatot.  

 ![Automatikus skálázás folyamatábrája](./media/monitoring-overview-autoscale/Autoscale_Overview_v4.png)

hello következő magyarázatot érvényes hello előző ábrán toohello adatot.   

## <a name="resource-metrics"></a>Erőforrás metrikáit
Erőforrások kibocsátás metrikákat, ezeket a mérési szabályok később dolgoznak. Metrikák keresztül különböző módszereket származnak.
Virtuálisgép-méretezési csoportok telemetriai adatokat az Azure diagnostics ügynökök használja, mivel a webes alkalmazások és a felhőszolgáltatások telemetriai közvetlenül származik hello Azure infrastruktúra. Néhány gyakran használt statisztikai közé tartozik a CPU-használat, a memória használata, a szál száma, a várólista hossza és a lemezek használata terén. Milyen telemetriai adatok használható listájáért lásd: [automatikus skálázás közös metrikák](insights-autoscale-common-metrics.md).

## <a name="custom-metrics"></a>Egyéni metrikák
Kihasználhatja a saját egyéni metrikákat, amelyek az alkalmazás esetleg kibocsátó. Ha konfigurálta a alkalmazás(ok) toosend metrikák tooApplication is élvezheti ezeket metrikák toomake döntést arról, hogy Insights tooscale vagy sem. 

## <a name="time"></a>Time
Ütemezésalapú szabályok UTC alapulnak. Meg kell adni az időzónájának megfelelően a szabályok beállítása során.  

## <a name="rules"></a>Szabályok
hello ábrán csak egy automatikus skálázási szabály, de ezek is. Létrehozhat összetett átfedő szabályokat a helyzetnek igény szerint.  Szabály típusa  

* **A metrika-alapú** – például a művelet végrehajtására, 50 % feletti CPU-használat esetén.
* **Időalapú** – például olyan webhook minden szombaton adott időzónában 8 am eseményindító.

Metrika-alapú szabályok mérésére alkalmazásterhelés, és adja hozzá, vagy távolítsa el a virtuális gépek terhelés alapján. Ütemezésalapú szabályok lehetővé teszik tooscale című a betöltési idő kombinációját, és szeretné, hogy tooscale előtt a lehetséges terhelés növelését, vagy csökkentse következik be.  

## <a name="actions-and-automation"></a>Műveletek és automatizálás
Szabályok is elindíthatja egy vagy több típusú műveleteket.

* **Skála** -skálázási virtuális gépek bejövő vagy kimenő
* **E-mailek** -küldjön e-mailek toosubscription rendszergazdák, a társadminisztrátoroknak és/vagy a megadott további e-mail cím
* **Webhook segítségével automatizálhatja** -hívás webhookokkal, melyek több összetett műveletek belül vagy kívül Azure elindítható. Azure-ban belül egy Azure Automation forgatókönyv, az Azure-függvény vagy az Azure Logic App is elindítható. Példa külső URL-cím kívül Azure-szolgáltatásokat, mint a Slackhez és Twilio tartalmaznak.

## <a name="autoscale-settings"></a>Automatikus skálázási beállításokat
Automatikus skálázás használja hello következő terminológia és struktúrája.

- Egy **automatikus skálázási beállítás** olvassa hello automatikus skálázás motor toodetermine e tooscale felfelé vagy lefelé. Egy vagy több profilok, hello célerőforrása és értesítés beállításaival kapcsolatos információkat tartalmaz.

    - Egy **automatikus skálázási profil** a: kombinációja

        - **kapacitás beállítás**, ami azt jelenti, hogy hello minimális, maximális és alapértelmezett értékei a példányok számát.
        - **szabályok**, amelyek mindegyike tartalmaz egy eseményindító (idő vagy metrika) és a méretezési művelet (felfelé vagy lefelé).
        - **Ismétlődés**, ami azt jelzi, ha automatikus skálázás kell a profil hatályba léptetni.

        Több profilok, amelyek lehetővé teszik különböző átfedő követelmények kiszolgálásához tootake lehet. Különböző automatikus skálázási profilok például is rendelkezik a különböző napszakokban hello hét nap.

    - A **beállítása** határozza meg, milyen értesítéseket jöjjön létre profilok hello automatikus skálázási beállítás egyik hello feltételeknek eleget tevő alapján automatikus skálázás esemény bekövetkezésekor. Automatikus skálázás értesítés egy vagy több e-mail címet, vagy hívások tooone vagy több webhook.


![Az Azure automatikus skálázási beállítás, a profil és a szabály struktúra](./media/monitoring-overview-autoscale/AzureResourceManagerRuleStructure3.png)

hello konfigurálható mezőket és leírások teljes listája megtalálható hello [automatikus skálázás REST API](https://msdn.microsoft.com/library/dn931928.aspx).

Kódminták lásd:

* [Speciális automatikus skálázás konfigurációs Resource Manager sablonok Virtuálisgép-méretezési készlet használata](insights-advanced-autoscale-virtual-machine-scale-sets.md)  
* [Automatikus skálázás REST API-n](https://msdn.microsoft.com/library/dn931953.aspx)

## <a name="horizontal-vs-vertical-scaling"></a>Vízszintes vagy függőleges skálázás
Automatikus skálázási csak arányosan vízszintesen, ez ("out") növelését vagy csökkenését ("") a Virtuálisgép-példányok száma hello.  Vízszintes rugalmasabb felhő helyzetben, toorun potenciálisan több ezer virtuális gépek toohandle terhelés lehetővé teszi.

Ezzel szemben függőleges skálázás nem azonos. Azt a memóriában tárolja a virtuális gépek azonos számú hello, de elérhetővé válnak a virtuális gépek ("Mentés") több vagy kevesebb ("le") hatékony hello. A memória, Processzor sebessége, lemezterület stb mérik.  További korlátozások függőleges skálázás rendelkezik. Függő hello rendelkezésre állását nagyobb hardverével, ezáltal gyorsan találatok száma a felső korlátja, és régiónként eltérőek lehetnek. Függőleges skálázás is általában egy virtuális gép toostop igényel, és indítsa újra.

További információkért lásd: [függőleges skálázása az Azure virtuális gép az Azure Automation szolgáltatásban](../virtual-machines/linux/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="methods-of-access"></a>Hozzáférési módok
Automatikus skálázás keresztül állíthat be

* [Azure Portal](insights-how-to-scale.md)
* [PowerShell](insights-powershell-samples.md#create-and-manage-autoscale-settings)
* [Többplatformos parancssori felület (CLI)](insights-cli-samples.md#autoscale)
* [Az Azure figyelő REST API-n](https://msdn.microsoft.com/library/azure/dn931953.aspx)

## <a name="supported-services-for-autoscale"></a>Az automatikus skálázás támogatott szolgáltatások
| Szolgáltatás | Séma & Docs |
| --- | --- |
| Web Apps |[Webalkalmazások skálázás](insights-how-to-scale.md) |
| Cloud Services |[Automatikus skálázás egy felhőalapú szolgáltatás](../cloud-services/cloud-services-how-to-scale.md) |
| Virtuális gépek: klasszikus |[Méretezési klasszikus virtuális gép rendelkezésre állási csoportok](https://blogs.msdn.microsoft.com/kaevans/2015/02/20/autoscaling-azurevirtual-machines/) |
| Virtuális gépek: Windows méretezési csoportok |[A Windows virtuálisgép-méretezési skálázás beállítása](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) |
| Virtuális gépek: Linux-skálázási készletekben |[A Linux virtuálisgép-méretezési skálázás beállítása](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) |
| Virtuális gépek: Windows – példa |[Speciális automatikus skálázás konfigurációs Resource Manager sablonok Virtuálisgép-méretezési készlet használata](insights-advanced-autoscale-virtual-machine-scale-sets.md) |

## <a name="next-steps"></a>Következő lépések
További információ az automatikus skálázás, toolearn használja az automatikus skálázás forgatókönyvek, előzőleg felsorolt hello, vagy tekintse meg a következő erőforrások toohello:

* [Az Azure a figyelő automatikus skálázás közös metrikák](insights-autoscale-common-metrics.md)
* [Ajánlott eljárások az Azure a figyelő automatikus skálázás](insights-autoscale-best-practices.md)
* [Használja az automatikus skálázási műveletek toosend e-mailek és a webhook riasztási értesítésekre](insights-autoscale-to-webhook-email.md)
* [Automatikus skálázás REST API-n](https://msdn.microsoft.com/library/dn931953.aspx)
* [Hibaelhárítási virtuális gép méretezési készlet automatikus skálázás](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)
