---
title: "az Azure tevékenységnapló hello aaaOverview |} Microsoft Docs"
description: "Ismerje meg, milyen hello Azure tevékenységnapló van, és hogyan használhatja az Azure-előfizetéshez belül előforduló toounderstand események."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c274782f-039d-4c28-9ddb-f89ce21052c7
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: johnkem
ms.openlocfilehash: cba79b7f6dc0833ef588382e763761fd77d43307
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-subscription-activity-with-hello-azure-activity-log"></a>Az Azure tevékenységnapló hello előfizetés figyelése
Hello **Azure tevékenységnapló** van egy előfizetési napló, amely történt az Azure-előfizetés szintű események betekintést nyújt. Ez magában foglalja az Azure Resource Manager működési adatokat tooupdates a szolgáltatás állapotával kapcsolatos események adatait számos. hello tevékenységnapló korábban hívták "Naplófájlok" vagy "Műveleti naplókat," hello felügyeleti kategória jelentések vezérlő-vezérlősík eseményeket az előfizetések óta. Az hello tevékenységnapló is meghatározható hello "mi, ki, és mikor" az esetleges írási műveleteket (PUT, POST, Törlés) végzett hello erőforrást az előfizetésében. Hello művelet és egyéb kapcsolódó tulajdonságainak hello állapotának értelmezni is lehet. hello tevékenységnapló nem tartalmaz olvasható (GET) vagy a klasszikus hello használó erőforrások / "RDFE" modell.

![Tevékenység naplók és a más típusú naplók ](./media/monitoring-overview-activity-logs/Activity_Log_vs_other_logs_v5.png)

1. ábra: Tevékenységi naplóit vagy más típusú naplók

hello tevékenységnapló eltér az [diagnosztikai naplók](monitoring-overview-of-diagnostic-logs.md). Tevékenységi naplóit hello műveletek hello (hello "vezérlő vezérlősík") kívül az erőforrás adatait adja meg. Diagnosztikai naplók az erőforrás által kibocsátott, és adja meg az erőforrás ("data vezérlősík" hello) hello műveletekre vonatkozó információk.

Események beolvasható a tevékenységnapló hello Azure-portálon CLI, PowerShell-parancsmagok használatával és az Azure figyelő REST API-t.


> [!WARNING]
> hello Azure tevékenységnapló van, amely elsősorban az Azure Resource Manager-tevékenységek. Nem követi nyomon a erőforrások hello klasszikus/RDFE modell használatával. Egyes klasszikus erőforrás állnak a proxy erőforrás-szolgáltató az Azure Resource Manager (például Microsoft.ClassicCompute). Ha Ön a szolgáltatóosztályokkal klasszikus erőforrástípus Azure Resource Manageren keresztül a proxy erőforrás-szolgáltatókat használ, hello tevékenységnapló hello műveletek jelennek meg. Ön a szolgáltatóosztályokkal klasszikus erőforrás hello klasszikus portál írja be vagy egyéb kívül hello Azure Resource Manager proxykat, a műveletek csak tárolja, amely hello műveletnaplóban. hello műveletnaplóban metódus csak hello klasszikus portál érhető el.
>
>

A következő videó introducing hello tevékenységnapló nézet hello.
> [!VIDEO https://channel9.msdn.com/Blogs/Seth-Juarez/Logs-John-Kemnetz/player]
> 
>

## <a name="categories-in-hello-activity-log"></a>A kategóriák hello műveletnapló
hello tevékenységnapló számos modulkategória közül adatokat tartalmazza. Az ezekben a kategóriákban hello sémákra részletes [ebben a cikkben találhat](monitoring-activity-log-schema.md). Ezek a következők:
* **Felügyeleti** -ebbe a kategóriába tartalmazza az összes hello rekord létrehozása, update, delete és művelet műveleteket Resource Manageren keresztül. Példát mutatunk be ebbe a kategóriába események típusai hello "virtuális gép létrehozása" és "hálózati biztonsági csoport törlése" minden felhasználó által végrehajtott műveletet, vagy az alkalmazás erőforrás-kezelővel egy adott erőforrástípus művelet van modellezve. Ha hello művelet típusa beírni, a művelet, vagy törlése hello rekordokat hello kezdő és a sikeres vagy sikertelen a művelet rögzíti hello felügyeleti kategória. hello felügyeleti kategória is bármely módosítások toorole-alapú hozzáférés-vezérlés egy előfizetésben.
* **Szolgáltatás állapota** -ebbe a kategóriába történt az Azure-szolgáltatás állapotának események hello rekordot tartalmaz. Ebbe a kategóriába tartozó mutatunk be hello eseménytípusra példa: "USA keleti régiója az SQL Azure tapasztal állásidő." Szolgáltatás állapotával kapcsolatos események térjen öt fajta: beavatkozás szükséges, támogatott helyreállítási, incidens, karbantartási, adatokat vagy biztonsági, és csak jelenik meg, ha egy erőforrást, amely akkor negatív hatással lehet hello esemény hello előfizetésben.
* **Riasztási** -ebbe a kategóriába Azure riasztások valamennyi aktiváláshoz hello rekordot tartalmaz. Ebbe a kategóriába tartozó mutatunk be hello eseménytípusra példa: "CPU % myVM lett több mint 80-as hello elmúlt 5 perc." Az Azure rendszereket rendelkezik egy riasztási koncepció--valamiféle szabály megadása, és értesítést kaphat, ha feltételek egyeznek meg, hogy a szabály. Minden alkalommal, amikor egy támogatott Azure riasztástípus "aktiválja," vagy hello feltételek fennállnak toogenerate értesítést, hello aktiválás rekord is fejlesztőre hello tevékenységnapló toothis kategóriáját.
* **Automatikus skálázás** -ebbe a kategóriába bármely események kapcsolódó toohello művelet hello automatikus skálázás motor minden automatikus skálázási beállításokat adta meg az előfizetés alapján hello rekord tartalmazza. Ebbe a kategóriába tartozó mutatunk be hello eseménytípusra példa, hogy "Automatikus skálázás felskálázott művelete nem sikerült." Használja az automatikus skálázás, automatikusan horizontális felskálázás vagy méretezni a hello támogatott erőforrástípus található példányok száma alapján idő nap és/vagy terhelés (mérték) adatok az automatikus skálázási beállítás használatával. Hello feltételek teljesülése esetén tooscale felfelé vagy lefelé, hello kezdő- és sikeres vagy sikertelen események tárolja, amely ezt a kategóriát.
* **A javaslat** -ebbe a kategóriába bizonyos erőforrástípusokra, például a webhelyek és az SQL Server-kiszolgálók az ajánlás eseményeket tartalmazza. Ezek az események javaslatok hogyan toobetter használják az erőforrásokat kínálnak. Az ilyen típusú események csak ha erőforrásokat, amelyek a kibocsátás javaslatokat kap.
* **A házirend, a biztonság és a Resource Health** – ezen kategóriák nem tartalmaz eseményeket, a jövőbeli használatra fenntartva.

## <a name="event-schema-per-category"></a>Esemény séma kategóriánként
[Ez a cikk toounderstand hello tevékenységnapló esemény séma kategóriánként témakörben talál.](monitoring-activity-log-schema.md)

## <a name="what-you-can-do-with-hello-activity-log"></a>Mire képes a hello műveletnapló
Az alábbiakban néhány hello dolgot hello tevékenységnapló teheti:

![Azure tevékenységnapló](./media/monitoring-overview-activity-logs/Activity_Log_Overview_v3.png)


* Lekérdezés, és tekintse meg hello **Azure-portálon**.
* [Riasztás létrehozása a tevékenységnapló esemény.](monitoring-activity-log-alerts.md)
* [Adatfolyamként tooan **Eseményközpont** ](monitoring-stream-activity-logs-event-hubs.md) egy külső szolgáltatás vagy az egyéni elemzési megoldások, például a Power bi szempontjából.
* Elemezze a Power bi használatával hello [ **Power bi tartalomcsomag**](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).
* [Mentse tooa **Tárfiók** archív vagy manuális ellenőrzést](monitoring-archive-activity-log.md). Megadhatja a hello megőrzési ideje (nap) hello segítségével **napló profil**.
* Lekérdezése a PowerShell-parancsmag, a parancssori felületen vagy a REST API-t.

## <a name="query-hello-activity-log-in-hello-azure-portal"></a>Lekérdezés hello tevékenységnapló a hello Azure-portálon
Hello Azure-portálon belül több helyen tekintheti meg a tevékenységnapló:
* Hello **tevékenységnapló panel**, hello bal oldali navigációs panelen a "Több szolgáltatások" tevékenységnapló hello keresve adható meg.
* Hello **figyelő panel**, amely alapértelmezés szerint a hello bal oldali navigációs panelen látható. Tevékenységnapló hello Azure figyelő panel egy szakaszában.
* Bármilyen olyan erőforrás **erőforráspanelen**, például a virtuális gép hello konfigurációs panel. hello tevékenységnapló hello szakaszokban a legtöbb a erőforrás paneleken lehet lehet, és kattintással automatikusan hello események szűrése toothose kapcsolódó toothat adott erőforrás.

Hello Azure-portálon a tevékenységnapló a mezők szerint szűrheti:
* TimeSpan - hello kezdési és befejezési ideje eseményeket.
* Kategória – hello eseménykategória fent leírt módon.
* -Előfizetés egy vagy több Azure-előfizetés nevét.
* Erőforráscsoport - egy vagy több erőforráscsoportokat előfizetésekben belül.
* Erőforrás (név) – egy adott erőforrás hello nevét.
* Erőforrástípus - erőforrás, például Microsoft.Compute/virtualmachines hello típusát.
* Művelet neve – az Azure Resource Manager művelet, például Microsoft.SQL/servers/Write hello neve.
* Súlyosság - hello súlyossági szint hello esemény (tájékoztatás, figyelmeztetés, hiba, kritikus).
* Az esemény - 'hívó"hello, vagy hello műveletet végre felhasználó által kezdeményezett.
* Nyissa meg a keresési - egy megnyitott keresési szövegmezőben, amely megkeresi az adott karakterlánc összes esemény található összes mezőhöz között.

Miután beállított szűrők csoportja, mentheti lekérdezésként, ha átállítására lenne szükség tooperform munkamenetei között fennállásának hello azonos lekérdezéseket az adott újra alkalmazza a jövőbeli hello szűrők. Is rögzítheti egy lekérdezés tooyour Azure irányítópult tooalways megtartása követheti a meghatározott események.

Kattintson az "Apply" a lekérdezés futtatása, és minden egyező események megjelenítése. Kattintson a minden esetben hello listáját jeleníti meg a hello az esemény összegzése, valamint, hogy az esemény teljes nyers JSON hello.

Még több power hello kattintva **naplófájl-keresési** ikonra, amely megjeleníti a tevékenységnapló adatokat hello [napló Analytics tevékenység Naplóelemzési megoldás](../log-analytics/log-analytics-activity.md). hello tevékenységnapló panel egy alapszintű szűrő/tallózási élménynek a naplókat, de a Naplóelemzési lehetővé teszi, hogy Ön toopivot lekérdezése, és az adatok megjelenítése hatékonyabb módon kínál.

## <a name="export-hello-activity-log-with-a-log-profile"></a>Hello tevékenységnapló napló profillal exportálása
A **napló profil** a tevékenységnapló exportálásának módját szabályozza. Napló profilt használ, konfigurálhatja:

* Ha hello tevékenységnapló kell küldeni (Tárfiók, vagy az Event Hubs)
* Esemény kategóriák (írási, törlés, művelete) kell küldeni. *hello szerinti "kategória" napló profilok és tevékenységnapló események nem egyezik. Hello napló profil, a "Kategória" hello művelet típust (írási, törlés, művelete) jelöli. Tevékenységnapló esetben hello "kategória" tulajdonság jelöli a hello forrás- vagy típusú eseményeket (például felügyeleti ServiceHealth, riasztás és több).*
* Mely régiókat (hely) exportálja. Győződjön meg arról, hogy tooinclude "globális,", mivel számos események hello tevékenységnapló globális események.
* Mennyi ideig hello tevékenységnapló rendszer meddig őrizze meg a Storage-fiók.
    - Egy nulla napos megőrzési azt jelenti, hogy a naplók végtelen tartanak. Ellenkező esetben a hello értéke lehet bármely 1 és 2147483647 között eltelt napok számát.
    - Ha a megőrzési házirend-beállításokat, de naplók tárolása a Storage-fiók le van tiltva, (például, ha csak az Event Hubs vagy OMS beállítások vannak jelölve), hello adatmegőrzési hatástalan.
    - Adatmegőrzési alkalmazott napi, így: hello napi (UTC) szerint végét, amelyik most már túl hello adatmegőrzési hello nap naplózza a rendszer törli. Például ha egy nap adatmegőrzési, ma hello nap hello elején hello hello nap előtt tegnapi naplók törlése akkor történik meg.

Használhatja a storage-fiók, vagy event hub névtér, amely nem része hello ugyanahhoz az előfizetéshez, hello egy kibocsátó naplókat. hello beállítás konfiguráló hello felhasználónak rendelkeznie kell hello megfelelő Szerepalapú hozzáférés tooboth előfizetések.

Ezek a beállítások hello "Exportálás" beállítás hello tevékenységnapló panelen hello portálon keresztül is konfigurálható. Akkor is konfigurálható programozott módon [Azure figyelő REST API használatával hello](https://msdn.microsoft.com/library/azure/dn931927.aspx), PowerShell-parancsmagok, vagy a parancssori felület. Előfizetés csak egy naplófájl-profillal rendelkezhet.

### <a name="configure-log-profiles-using-hello-azure-portal"></a>Konfigurálja a napló profilra hello Azure-portál használatával
Adatfolyam-hello tevékenységnapló tooan Eseményközpont, vagy beállítással hello "Exportálás" hello Azure-portálon a letölthetők a Storage-fiók.

1. Keresse meg a toohello **tevékenységnapló** panel bal oldalán található hello portal hello a hello menü segítségével.

    ![Keresse meg a napló tooActivity portálon](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Kattintson a hello **exportálása** hello panel felső hello gombra.

    ![A portál Exportálás gomb](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. A megjelenő panelen hello közül választhat:  
  * régiók, amelynek szeretné tooexport események
  * a Tárfiók toowhich toosave események milyen hello
  * Ezeket az eseményeket, megőrzés tooretain kívánt napok száma hello. A 0 nap beállításnak hello naplók végtelen megőrzi.
  * hello a Service Bus Namespace, amelyben az Eseményközpont toobe létrehozott az adatfolyamként történő ezeket az eseményeket szeretné.

     ![Exportálás tevékenységnapló panel](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Kattintson a **mentése** toosave ezeket a beállításokat. hello-beállítások vannak azonnal kell alkalmazott tooyour előfizetés.

### <a name="configure-log-profiles-using-hello-azure-powershell-cmdlets"></a>Hello Azure PowerShell-parancsmagok használatával napló profilok konfigurálása
#### <a name="get-existing-log-profile"></a>Meglévő napló profil beolvasása
```
Get-AzureRmLogProfile
```

#### <a name="add-a-log-profile"></a>Napló profil hozzáadása
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

| Tulajdonság | Szükséges | Leírás |
| --- | --- | --- |
| Név |Igen |A napló profil neve. |
| StorageAccountId |Nem |Erőforrás-azonosítója, hello Tárfiók toowhich hello tevékenységnapló menteni kell. |
| serviceBusRuleId |Nem |Service Bus-Szabályazonosító hello Service Bus-névtér toohave az event hubs létrehozott szeretné. Egy karakterlánc, ebben a formátumban: `{service bus resource ID}/authorizationrules/{key name}`. |
| Helyek |Igen |Régiók, amelynek szeretné toocollect tevékenységnapló események vesszővel tagolt listája. |
| retentionInDays |Igen |Az eseményeket meg kell őrizni, 1 és 2147483647 közötti napok számát. A nulla érték hello naplók határozatlan ideig tárolja (végtelen). |
| Kategóriák |Nem |Be kell esemény kategóriák vesszővel tagolt listája. Lehetséges értékek a következők: Olvasás, törlés és művelet. |

#### <a name="remove-a-log-profile"></a>Napló-profil eltávolítása
```
Remove-AzureRmLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-hello-azure-cross-platform-cli"></a>Napló-profilok konfigurálása a hello Azure platformfüggetlen parancssori Felülettel
#### <a name="get-existing-log-profile"></a>Meglévő napló profil beolvasása
```
azure insights logprofile list
```
```
azure insights logprofile get --name my_log_profile
```
Hello `name` tulajdonság a napló profil hello nevének kell lennie.

#### <a name="add-a-log-profile"></a>Napló profil hozzáadása
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

| Tulajdonság | Szükséges | Leírás |
| --- | --- | --- |
| név |Igen |A napló profil neve. |
| storageId |Nem |Erőforrás-azonosítója, hello Tárfiók toowhich hello tevékenységnapló menteni kell. |
| serviceBusRuleId |Nem |Service Bus-Szabályazonosító hello Service Bus-névtér toohave az event hubs létrehozott szeretné. Egy karakterlánc, ebben a formátumban: `{service bus resource ID}/authorizationrules/{key name}`. |
| Helyek |Igen |Régiók, amelynek szeretné toocollect tevékenységnapló események vesszővel tagolt listája. |
| retentionInDays |Igen |Az eseményeket meg kell őrizni, 1 és 2147483647 közötti napok számát. A nulla érték hello naplók határozatlan ideig tárolja (végtelen). |
| Kategóriák |Nem |Be kell esemény kategóriák vesszővel tagolt listája. Lehetséges értékek a következők: Olvasás, törlés és művelet. |

#### <a name="remove-a-log-profile"></a>Napló-profil eltávolítása
```
azure insights logprofile delete --name my_log_profile
```

## <a name="next-steps"></a>Következő lépések
* [További tudnivalók hello tevékenységnapló (korábbi nevén naplók)](../azure-resource-manager/resource-group-audit.md)
* [Adatfolyam-hello Azure tevékenységnapló tooEvent hubok](monitoring-stream-activity-logs-event-hubs.md)
