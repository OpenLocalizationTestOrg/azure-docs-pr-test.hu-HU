---
title: "szerepkörök, engedélyek és biztonsági Azure megfigyelővel használatába aaaGet |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure figyelő beépített szerepkörök és engedélyek toorestrict hozzáférés toomonitoring erőforrásokat."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 2686e53b-72f0-4312-bcd3-3dc1b4a9b912
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: johnkem
ms.openlocfilehash: 44fdf2a697050309480dfc164ebab51445b19bc8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-roles-permissions-and-security-with-azure-monitor"></a>Ismerkedés a szerepkörök, engedélyek és biztonságának és az Azure-figyelő
Sok toostrictly szükséges hozzáférési toomonitoring adatok és beállítások szabályozzák. Például, ha vannak olyan dolgozó kizárólag figyelése (a támogatási szakértők, devops mérnökök) csoport tagjai, vagy ha egy felügyelt szolgáltató használja, érdemes lehet őket elérni a figyelési adatok közben korlátozó saját képességét toocreate tooonly toogrant, módosít, vagy törli az erőforrást. Ez a cikk bemutatja, hogyan tooquickly alkalmazni egy beépített figyelési RBAC szerepkör tooa felhasználói az Azure-ban vagy a felhasználókat, akiknek korlátozott felügyeleti engedélyekkel kell a saját egyéni szerepkör létrehozása. Majd az Azure-figyelő kapcsolódó erőforrások biztonsági szempontjai mutatjuk be, és hogyan korlátozhatja a hozzáférést toohello adatokat tartalmaznak.

## <a name="built-in-monitoring-roles"></a>Beépített figyelési szerepkörök
Azure figyelő beépített szerepkörök felelősek tervezett toohelp korlát hozzáférés tooresources továbbra is engedélyezze azokat egy előfizetésben infrastruktúra tooobtain, és konfigurálja a szükséges hello adatokhoz. Az Azure két out-of-az-box szerepkörök biztosít: A figyelési olvasó és a figyelés közreműködő.

### <a name="monitoring-reader"></a>Figyelési olvasó
Hello figyelési olvasó szerepkört személyek megtekintheti összes figyelési adatokat egy előfizetésben, de nem minden erőforrás módosítása vagy szerkesztheti beállítások kapcsolódó toomonitoring erőforrásokat. A szerepkör a vállalatnál, például a támogatási szolgálathoz vagy a műveletek mérnökök, ki kell tudni toobe megfelelő:

* Figyelési irányítópult megtekintése hello portálon, és a saját személyes figyelési irányítópultot létrehozni.
* Lekérdezés hello segítségével metrikáihoz [Azure figyelő REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx), [PowerShell-parancsmagok](insights-powershell-samples.md), vagy [platformfüggetlen parancssori felület](insights-cli-samples.md).
* Lekérdezés hello tevékenységnapló hello portál, az Azure figyelő REST API-t, a PowerShell-parancsmagok vagy a platformfüggetlen parancssori felület használatával.
* Nézet hello [diagnosztikai beállítások](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) erőforrás.
* Nézet hello [profilt naplózni](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile) az előfizetéshez.
* Automatikus skálázási beállítások megtekintéséhez.
* Riasztási tevékenység megtekintése és a beállítások.
* Az Application Insights adatokat, és AI Analytics adatainak megtekintéséhez.
* Keresési Naplóelemzés (OMS) adatai hello munkaterület használati adatokat is beleértve.
* Naplóelemzés (OMS) felügyeleti csoportoknak a megtekintése.
* Hello Naplóelemzés (OMS) keresési séma beolvasása.
* Lista Naplóelemzés (OMS) eszközintelligencia csomagokat.
* Kérje le, majd Naplóelemzés (OMS) mentett keresések hajtható végre.
* Hello Naplóelemzés (OMS) tárolási konfigurációt beolvasása.

> [!NOTE]
> Ez a szerepkör nem ad adatfolyamként továbbított tooan eseményközpont vagy a storage-fiókban tárolt olvasási hozzáférés toolog adatokat. [Lásd az alábbi](#security-considerations-for-monitoring-data) toothese erőforrások eléréséhez konfigurálásával kapcsolatos információkat.
> 
> 

### <a name="monitoring-contributor"></a>A közreműködői figyelése
Hozzárendelt személyek hello figyelési közreműködői szerepkört is összes figyelési adatok megtekintése az előfizetés és hozzon létre vagy figyelési beállítások azonban nem tudja módosítani összes erőforrását. Ez a szerepkör felülbírálja a hello figyelési olvasó szerepkört, és egy szervezet figyelési team vagy felügyelt szolgáltatók példányaik, továbbá a fenti toohello engedélyeket is toobe tudni tagjai számára megfelelő:

* Tegye közzé a figyelési irányítópult megosztott irányítópultként.
* Állítsa be [diagnosztikai beállítások](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) egy resource.* számára
* Set hello [profilt naplózni](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile) egy subscription.* számára
* Riasztási tevékenység és a beállítások megadásához.
* Az Application Insights webalkalmazás-tesztek és összetevők létrehozása.
* Naplóelemzés (OMS) munkaterületén a megosztott kulcsok szükségesek.
* Engedélyezi vagy letiltja eszközintelligencia csomagok Naplóelemzés (OMS).
* Hozzon létre és törlése, Naplóelemzés (OMS) mentett keresések hajtható végre.
* Hozzon létre, és törölni hello Naplóelemzés (OMS).

* felhasználói külön is engedéllyel kell rendelkezni ListKeys hello célként megadott erőforrás (tárolási fiók vagy esemény hub névtér) tooset napló profil vagy diagnosztikai beállítást.

> [!NOTE]
> Ez a szerepkör nem ad adatfolyamként továbbított tooan eseményközpont vagy a storage-fiókban tárolt olvasási hozzáférés toolog adatokat. [Lásd az alábbi](#security-considerations-for-monitoring-data) toothese erőforrások eléréséhez konfigurálásával kapcsolatos információkat.
> 
> 

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>Figyelési engedélyek és egyéni RBAC-szerepkörök
Ha beépített szerepkörök fent hello nem igényeihez hello pontos a csoport, akkor [hozzon létre egy egyéni RBAC szerepkör](../active-directory/role-based-access-control-custom-roles.md) részletesebb engedélyekkel. Az alábbiakban hello közös Azure RBAC-figyelő műveletek azok leírásait tartalmazza.

| Művelet | Leírás |
| --- | --- |
| Microsoft.Insights/AlertRules/[Read, írási, törlési] |Olvasási, írási és törlési riasztási szabályok. |
| Microsoft.Insights/AlertRules/Incidents/Read |A riasztási szabályok listázása incidensek (hello riasztási szabály alatt indított előzmények). Ez csak érvényes toohello portálon. |
| Microsoft.Insights/AutoscaleSettings/[Read, írási, törlési] |Olvasási, írási és törlési automatikus skálázási beállításokat. |
| Microsoft.Insights/DiagnosticSettings/[Read, írási, törlési] |Olvasási, írási és törlési diagnosztikai beállítások. |
| Microsoft.Insights/eventtypes/digestevents/Read |Ez az engedély szükséges tooActivity naplók hello portálon keresztül elérő felhasználók szükség. |
| Microsoft.Insights/eventtypes/values/Read |Tevékenységnapló események (felügyeleti események) egy előfizetésben listában. Ez az engedély alkalmazható tooboth programozott és portál toohello tevékenységnapló. |
| Microsoft.Insights/LogDefinitions/Read |Ez az engedély szükséges tooActivity naplók hello portálon keresztül elérő felhasználók szükség. |
| Microsoft.Insights/MetricDefinitions/Read |Olvassa el a metrikai meghatározásainak (erőforrás elérhető metrika típusok listája). |
| Microsoft.Insights/Metrics/Read |Egy erőforrás olvasni. |

> [!NOTE]
> Tooalerts, a diagnosztikai beállításokat és a metrikák elérhető erőforrás megköveteli, hogy hello felhasználó rendelkezik-e olvasási hozzáférést toohello erőforrástípustól és a hatókör erőforrás. Létrehozás ("write"), hogy archívumokat tooa tárolási fiók vagy adatfolyamok tooevent hubok igényel hello felhasználói tooalso diagnosztikai beállítás vagy a napló profil engedélye ListKeys hello cél erőforráson.
> 
> 

Például táblázat felett hello segítségével létrehozhat egy egyéni RBAC szerepkör egy "tevékenység napló olvasó" ehhez hasonló:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzureRmRoleDefinition -Role $role 
```

## <a name="security-considerations-for-monitoring-data"></a>Figyelési adatok biztonsági szempontjai
Figyelési adatok – különösen naplófájlok – tartalmazhatnak bizalmas adatokat, például az IP-címek vagy felhasználó neve. Három alapvető képernyőn tartalmaz figyelési adatok az Azure-ból:

1. hello tevékenységnapló, amely minden vezérlő-vezérlősík műveleteket ismerteti az Azure-előfizetéshez.
2. Diagnosztikai naplók, amelyek az erőforrás által kibocsátott naplókat.
3. Metrika, amely erőforrások által kibocsátott.

Ezek az adattípusok három tárolhatja a storage-fiók, vagy folyamatos átviteli tooEvent Hub, amelyek mindkettő általános célú Azure-erőforrások. Mivel ezek az általános célú erőforrások, létrehozása, törlése és hozzájuk férni egy olyan privileged művelet általában a rendszergazda számára van fenntartva. Javasoljuk, hogy a következő eljárásokkal kapcsolatos megfigyelési erőforrások tooprevent visszaélés hello használata:

* A figyelési adatok dedikált tárolási fiók használata. Ha tooseparate figyelési adatok több tárfiókot az kell, ne ossza meg a storage-fiókok között figyelés használatát, és előfordulhat, hogy nem figyelési adatok, ez véletlenül adjon azok, akik csak kell adatelérés toomonitoring (pl.. egy külső SIEM) toonon figyelési adatok eléréséhez.
* Használjon egy dedikált a Service Bus vagy az Eseményközpont kiválasztásával névtér összes diagnosztikai beállítások között ugyanabból az okból a fenti hello.
* Korlátozza a hozzáférést toomonitoring kapcsolódó tárfiókok vagy az event hubs egy külön erőforráscsoportban található tartja és [hatókört használja](../active-directory/role-based-access-control-what-is.md#basics-of-access-management-in-azure) a figyelési szerepkörök toolimit elérhetővé tooonly erőforráscsoporthoz.
* Soha nem engedélyt hello ListKeys storage-fiókok vagy az event hubs előfizetési hatókört, amikor a felhasználó csak hozzáférés toomonitoring adatokra van szüksége. Ehelyett adjon az ezen engedélyek toohello felhasználó egy erőforrás vagy az erőforráscsoport (ha van egy dedikált figyelési erőforráscsoport) hatókör.

### <a name="limiting-access-toomonitoring-related-storage-accounts"></a>Korlátozó hozzáférési toomonitoring kapcsolatos storage-fiókok
Amikor egy felhasználó vagy alkalmazás toomonitoring adatok olyan tárfiókban kell elérni, érdemes [egy fiók SAS létre](https://msdn.microsoft.com/library/azure/mt584140.aspx) hello tárfiók, amely tartalmazza a szolgáltatási szint csak olvasási hozzáféréssel tooblob tárolóval figyelési adatokat. A PowerShellben ez látható:

```powershell
$context = New-AzureStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzureStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

Majd adhat a hello token toohello entitást, amelyet a tooread adott tárfiókból, és azt listában, és olvassa el az összes BLOB storage-fiókhoz tartozó.

Azt is megteheti Ha toocontrol kell ezt az engedélyt az RBAC, meg lehet adni, hogy entitás hello Microsoft.Storage/storageAccounts/listkeys/action engedélyt, hogy adott tárfiókok. Erre akkor szükség a felhasználók, akik toobe képes tooset diagnosztikai beállítás vagy a napló profil tooarchive tooa tárfiók. Például létrehozhatja a következő egyéni RBAC szerepkör egy felhasználó vagy alkalmazás, amely csak a tooread egy tárfiókot az kell hello:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get hello storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzureRmRoleDefinition -Role $role 
```

> [!WARNING]
> hello ListKeys engedély lehetővé teszi, hogy a hello felhasználói toolist hello elsődleges és másodlagos tárfiók kulcsait. Ezek a kulcsok hello felhasználó kap minden aláírt engedélyeket (olvasás, írás, blobok létrehozása, törlése, blobok stb.) összes aláírt szolgáltatások (blob, várólista, tábla, fájl), hogy a tárfiók. Egy fiók SAS, ha lehetséges, a fent leírt használatát javasoljuk.
> 
> 

### <a name="limiting-access-toomonitoring-related-event-hubs"></a>Korlátozó hozzáférési toomonitoring kapcsolatos az event hubs
Hasonló mintát gyűjtheti az event hubs, de először meg kell toocreate egy dedikált figyelési engedélyezési szabályt. Ha azt szeretné, hogy toogrant access tooan alkalmazást, amelyet csak a toolisten toomonitoring kapcsolatos eseményközpontokhoz, a hello, a következő:

1. Megosztott hozzáférési házirend létrehozása a figyelési adatok csak a figyelési JOGCÍMEKKEL rendelkező és folyamatos létrehozott hello esemény hub(s). Ezt megteheti a hello portálon. Például akkor lehet, hogy neki "monitoringReadOnly." Ha lehetséges érdemes toogive, amely közvetlenül toohello fogyasztói kulcs, és hagyja ki a hello tovább.
2. Ha hello fogyasztói toobe képes tooget hello kulcs alkalmi van szüksége, adja meg a hello felhasználói hello ListKeys művelet adott eseményközpont. Ez egyben toobe képes tooset diagnosztikai beállításának kell, és jelentkezzen profil toostream tooevent hubok felhasználók számára szükséges. Például előfordulhat, hogy létre RBAC-szabályt:
   
   ```powershell
   $role = Get-AzureRmRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get hello key toolisten tooan event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzureRmRoleDefinition -Role $role 
   ```

## <a name="next-steps"></a>Következő lépések
* [További tudnivalók a Szerepalapú és engedélyeket az erőforrás-kezelőben](../active-directory/role-based-access-control-what-is.md)
* [Olvassa el az Azure-ban figyelési hello áttekintése](monitoring-overview.md)

