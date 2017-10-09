---
title: "Erőforrás-kezelő szolgáltató műveletek aaaAzure |} Microsoft Docs"
description: "Részletek hello hello Microsoft Azure Resource Manager erőforrás-szolgáltató elérhető műveleteinek"
services: active-directory
documentationcenter: 
author: jboeshart
manager: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: jaboes
ms.openlocfilehash: 2d2f912ecbade335667d68fdc42ce03a2930a0eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Az Azure Resource Manager erőforrás-szolgáltató üzemeltetése

Ez a dokumentum minden Microsoft Azure Resource Manager erőforrás-szolgáltató elérhető hello műveleteinek listázása. Ezek használhatók a egyéni szerepkörök tooprovide részletes szerepköralapú hozzáférés-vezérlést (RBAC) engedélyek tooresources az Azure-ban. Ne feledje, ez nem az átfogó listáját, és műveletek előfordulhat, hogy kell hozzáadni vagy eltávolítani, mert egyes szolgáltatók frissül. Művelet karakterláncok kövesse hello formátuma `Microsoft.<ProviderName>/<ChildResourceType>/<action>`. Az átfogó és aktuális listáját használja a `Get-AzureRmProviderOperation` (a PowerShell) vagy `azure provider operations show` (az Azure CLI) az Azure erőforrás-szolgáltató toolist működésére.

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

| Művelet | Leírás |
|---|---|
|/ configuration/művelet|Frissítések bérlő konfigurációjához.|
|/ services/művelet|A szolgáltatáspéldány hello bérlői frissíti.|
|/ configuration/írása|Létrehoz egy bérlői konfigurációt.|
|/Configuration/Read|Beolvassa a bérlő konfigurációjához hello.|
|/ services/írása|Létrehoz egy szolgáltatáspéldány hello bérlő.|
|/Services/Read|Hello szolgáltatás példányainak hello bérlő beolvasása.|
|/Services/DELETE|Törli a szolgáltatáspéldány hello-bérlőben.|
|/Services/servicemembers/Action|Létrehoz egy tag szolgáltatáspéldány hello szolgáltatásban.|
|/Services/servicemembers/Read|Beolvassa a tag szolgáltatáspéldány hello hello szolgáltatásban.|
|/Services/servicemembers/DELETE|Törli a tag példánya hello szolgáltatásban.|
|/Services/servicemembers/Alerts/Read|Egy tag hello riasztások beolvassa.|
|/Services/Alerts/Read|Egy szolgáltatás hello riasztások beolvasása.|
|/Services/Alerts/Read|Egy szolgáltatás hello riasztások beolvasása.|

## <a name="microsoftadvisor"></a>Microsoft.Advisor

| Művelet | Leírás |
|---|---|
|/ generateRecommendations/művelet|Ajánlatok generálása|
|/ suppressions/művelet|Suppressions hoz létre, frissítések|
|/ regisztrációs/művelet|A Microsoft Advisor hello hello az előfizetés regisztrálása|
|/generateRecommendations/Read|Lekérdezi készítése javaslatok állapota|
|/recommendations/Read|Javaslatok beolvasása|
|/suppressions/Read|Lekérdezi a suppressions|
|/suppressions/DELETE|Tiltási törlése|

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

| Művelet | Leírás |
|---|---|
|/Servers/Read|Lekéri hello információkat hello Analysis-kiszolgálóhoz megadott.|
|/ kiszolgálók/írása|Létrehozza vagy frissíti az hello megadott elemzési kiszolgáló.|
|/Servers/DELETE|Törlések hello Analysis-kiszolgálóhoz.|
|/Servers/suspend/Action|Felfüggeszti a hello Analysis-kiszolgálóhoz.|
|/Servers/Resume/Action|Folytatása hello Analysis-kiszolgálóhoz.|
|/ kiszolgálók/checkNameAvailability<br>/ művelet|Ellenőrzi, hogy a megadott Analysis Server neve érvényes, és nincs használatban.|

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

| Művelet | Leírás |
|---|---|
|/ checkNameAvailability/művelet|Ellenőrzi, hogy a megadott szolgáltatásnév érhető el|
|/ regisztrációs/művelet|Előfizetés regisztrálása a Microsoft.ApiManagement erőforrás-szolgáltató|
|/ unregister/művelet|Előfizetés regisztrációjának Microsoft.ApiManagement erőforrás-szolgáltató|
|/ service/írása|Az API Management szolgáltatást egy új példányának létrehozása|
|/Service/Read|Olvassa el a metaadatokat az API Management szolgáltatáspéldány|
|/Service/DELETE|Törli az API Management Service-példány|
|/Service/updatehostname/Action|A telepítő, frissítésére, vagy távolítsa el az API Management szolgáltatás egyéni tartománynevek|
|/Service/uploadcertificate/Action|Az API Management szolgáltatás SSL-tanúsítvány feltöltése|
|/Service/Backup/Action|Biztonsági mentési API Management szolgáltatás toohello megadott tároló a felhasználó a megadott tárfiók|
|/Service/restore/Action|Állítsa vissza API Management szolgáltatás a felhasználó által megadott tárfiók hello megadott tárolóhoz|
|/Service/managedeployments/Action|Módosítsa a Termékváltozat jegyek, hozzáadása regionális egy szolgáltatás üzemelő példányainak API Management|
|/Service/getssotoken/Action|Egyszeri bejelentkezési jogkivonatot, amely az API Management szolgáltatás örökölt portáljára rendszergazdaként használt toologin beolvasása|
|/Service/applynetworkconfigurationupdates/Action|Frissítések hello Microsoft.ApiManagement erőforrásaihoz, a virtuális hálózati toopick hálózati beállítások frissítése.|
|/Service/operationresults/Read|Hosszú ideig futó művelet aktuális állapotának beolvasása|
|/Service/networkStatus/Read|Hello hálózati hozzáférési erőforrások állapotának beolvasása.|
|/Service/loggers/Read|Figyelő szoftverek listája vagy az naplózó részleteinek beolvasása|
|/Service/loggers/Write|Adja hozzá az új naplózó vagy meglévő naplózó részleteinek frissítése|
|/Service/loggers/DELETE|Távolítsa el a meglévő naplózó|
|/Service/Users/Read|A regisztrált felhasználók listáját, vagy felhasználói fiók adatainak lekérése|
|/Service/Users/Write|Új felhasználó vagy egy meglévő felhasználó frissítés fiókadatok regisztrálása|
|/Service/Users/DELETE|Távolítsa el a felhasználói fiók|
|/Service/Users/generateSsoUrl/Action|Egyszeri bejelentkezési URL-címet létrehozni. hello URL-cím lehet használt tooaccess felügyeleti portál|
|/Service/Users/Subscriptions/Read|Felhasználói előfizetések listájának beolvasása|
|/Service/Users/keys/Read|Felhasználói kulcsok listájának beolvasása|
|/Service/Users/groups/Read|Felhasználói csoportok listájának beolvasása|
|/Service/tenant/operationResults/Read|A művelet eredménye listája vagy az beszerzése az adott művelet eredménye|
|/Service/tenant/Policy/Read|Házirend-konfigurációt hello bérlő beszerzése|
|/Service/tenant/Policy/Write|Házirend-konfiguráció hello bérlő számára|
|/Service/tenant/Policy/DELETE|Hello bérlő házirend-konfiguráció eltávolítása|
|/Service/tenant/Configuration/Save/Action|Hoz létre a véglegesítési konfigurációs pillanatkép toohello megadott fiókirodai hello tárházban|
|/Service/tenant/Configuration/Deploy/Action|Futtatja a szolgáltatástelepítési feladat tooapply módosítások hello megadott git fiókirodai toohello konfigurációs adatbázisában.|
|/Service/tenant/Configuration/Validate/Action|Ellenőrzi az hello megadott git fiókirodai módosítások|
|/Service/tenant/Configuration/operationResults/Read|A művelet eredménye listája vagy az beszerzése az adott művelet eredménye|
|/Service/tenant/Configuration/syncState/Read|Az utolsó git szinkronizálás állapotának beolvasása|
|/Service/tenant/Access/Read|Szerezze be a bérlő hozzáférési adatai|
|/Service/tenant/Access/Write|Bérlői hozzáférési információ részleteinek frissítése|
|/Service/tenant/Access/regeneratePrimaryKey/Action|Elsődleges elérési kulcs újragenerálása|
|/Service/tenant/Access/regenerateSecondaryKey/Action|Másodlagos elérési kulcs újragenerálása|
|/Service/identityProviders/Read|Az identitás-szolgáltatóktól vagy identitásszolgáltató Get részleteinek listáját beszerzése|
|/Service/identityProviders/Write|Hozzon létre egy új identitásszolgáltató vagy a frissítés részleteit egy meglévő identitásszolgáltató|
|/Service/identityProviders/DELETE|Távolítsa el a meglévő identitásszolgáltató|
|/Service/Subscriptions/Read|A termék előfizetések listáját, vagy a részletek a termék előfizetés|
|/Service/Subscriptions/Write|Egy meglévő felhasználó tooan meglévő termék előfizetés, vagy frissítse a meglévő előfizetés részletei. Ez a művelet lehet használt toorenew előfizetés|
|/Service/Subscriptions/DELETE|Előfizetés törlése. Ez a művelet lehet használt toodelete előfizetés|
|/Service/Subscriptions/regeneratePrimaryKey/Action|Előfizetés elsődleges kulcs újragenerálása|
|/Service/Subscriptions/regenerateSecondaryKey/Action|Előfizetés másodlagos kulcs újragenerálása|
|/Service/backends/Read|Háttérkiszolgálókon listáját, vagy a részletek a háttér|
|/Service/backends/Write|Adja hozzá egy új háttér vagy a meglévő háttér részleteinek frissítése|
|/Service/backends/DELETE|Távolítsa el a létező háttérrendszerek|
|/Service/APIs/Read|Szerezze be az összes regisztrált API-k vagy Get részleteit API listát|
|/Service/APIs/Write|Hozzon létre új API-JÁVAL vagy a meglévő API részleteinek frissítése|
|/Service/APIs/DELETE|Távolítsa el a meglévő API|
|/Service/APIs/Policy/Read|Ezzel a házirend konfigurációs adatokat az API-hoz|
|/Service/APIs/Policy/Write|Állítsa be a házirend-konfigurációs adatait az API-hoz|
|/Service/APIs/Policy/DELETE|Távolítsa el a házirend-konfigurációs API|
|/Service/APIs/Operations/Read|A meglévő API műveletek listájának vagy részletes API művelet|
|/Service/APIs/Operations/Write|Új API-művelet létrehozni vagy frissíteni a meglévő API-művelet|
|/Service/APIs/Operations/DELETE|Távolítsa el a meglévő API-művelet|
|/Service/APIs/Operations/Policy/Read|Ezzel a házirend konfigurációs adatokat művelet|
|/Service/APIs/Operations/Policy/Write|Adja meg a művelet házirend konfigurációs részleteit|
|/Service/APIs/Operations/Policy/DELETE|Távolítsa el a házirend-konfigurációt a művelet|
|/Service/Products/Read|Termékek listáját, vagy a részletek a termék|
|/Service/Products/Write|Új termék létrehozása vagy meglévő termékadatok frissítése|
|/Service/Products/DELETE|Távolítsa el a meglévő termék|
|/Service/Products/Subscriptions/Read|Termék előfizetések listájának beolvasása|
|/Service/Products/APIs/Read|Szerezze be az API-k hozzáadott tooexisting termék listát|
|/Service/Products/APIs/Write|Adja hozzá a meglévő API tooexisting termék|
|/Service/Products/APIs/DELETE|Távolítsa el a meglévő API a meglévő termék|
|/Service/Products/Policy/Read|Házirend-konfigurációt meglévő termék beszerzése|
|/Service/Products/Policy/Write|A termék meglévő házirend-konfiguráció|
|/Service/Products/Policy/DELETE|Távolítsa el a házirend-konfigurációt a meglévő termék|
|/Service/Products/groups/Read|Termék társított fejlesztői csoportok listájának beolvasása|
|/Service/Products/groups/Write|Meglévő fejlesztői csoport társítása meglévő termék|
|/Service/Products/groups/DELETE|Meglévő termék társítás meglévő fejlesztői csoport törlése|
|/Service/openidConnectProviders/Read|Szerezze be az OpenID Connect-szolgáltatókkal vagy az OpenID Connect szolgáltató Get részleteit listát|
|/Service/openidConnectProviders/Write|Hozzon létre egy új OpenID Connect szolgáltató vagy a frissítés adatait egy meglévő OpenID Connect-szolgáltató|
|/Service/openidConnectProviders/DELETE|Távolítsa el a meglévő OpenID Connect szolgáltató|
|/Service/Certificates/Read|A tanúsítványok listájának vagy a tanúsítvány adatainak lekérése|
|/Service/Certificates/Write|Új tanúsítvány felvétele|
|/Service/Certificates/DELETE|Távolítsa el a meglévő tanúsítvány|
|/Service/Properties/Read|Beolvassa az összes tulajdonság listáját, és lekérdezi a megadott tulajdonság részleteit|
|/Service/Properties/Write|Új tulajdonság létrehozása vagy frissítése a megadott tulajdonság értéke|
|/Service/Properties/DELETE|Eltávolítja a meglévő tulajdonság|
|/Service/groups/Read|Csoportok vagy egy csoport lekérdezi részleteinek listáját|
|/Service/groups/Write|Új csoport létrehozása vagy meglévő csoport részleteinek frissítése|
|/Service/groups/DELETE|Távolítsa el a meglévő csoporthoz|
|/Service/groups/Users/Read|Csoport felhasználók listájának beolvasása|
|/Service/groups/Users/Write|Meglévő felhasználói tooexisting csoport hozzáadása|
|/Service/groups/Users/DELETE|Távolítsa el a meglévő felhasználói meglévő csoportból|
|/Service/authorizationServers/Read|Engedélyezési kiszolgálók listája vagy az engedélyezési kiszolgáló adatait az beszerzése|
|/Service/authorizationServers/Write|Hozzon létre egy új hitelesítési kiszolgáló vagy egy meglévő engedélyezési kiszolgáló frissítés részletei|
|/Service/authorizationServers/DELETE|Távolítsa el a meglévő engedélyezési kiszolgáló|
|/Service/Reports/bySubscription/Read|A jelentés-előfizetés összesítve beolvasása.|
|/Service/Reports/byRequest/Read|Jelentési adatok kérelmek|
|/Service/Reports/byOperation/Read|Műveletek összesítve jelentés megtekintése|
|/Service/Reports/byGeo/Read|Földrajzi régió összesítve jelentés megtekintése|
|/Service/Reports/byUser/Read|A jelentés összesíti a fejlesztők beolvasása.|
|/Service/Reports/byTime/Read|A jelentés megtekintése időszakokra összesítve|
|/Service/Reports/byApi/Read|API-k összesítve jelentés megtekintése|
|/Service/Reports/byProduct/Read|A jelentés termékek összesítve beolvasása.|

## <a name="microsoftappservice"></a>Microsoft.AppService

| Művelet | Leírás |
|---|---|
|/ appidentities/Olvasás|Beolvasása hello hello átjáró regisztrált erőforrás (webhely).|
|/ appidentities/írása|Létrehoz egy új alkalmazás-azonosító.|
|/ appidentities/törlése|Törli a meglévő alkalmazás identitás.|
|/deploymenttemplates/listMetadata/Action|Hello API-alkalmazás csomag társított felhasználói felület metaadatokat tartalmazza.|
|/deploymenttemplates/Generate/Action|A központi telepítési sablont tooprovision API-alkalmazás példányokat adja vissza.|
|/ átjárók/Olvasás|Hello átjárópéldány értéket ad vissza.|
|/ átjárók/írása|Létrehoz egy új átjárót, vagy frissíti a meglévőt.|
|/ átjárók/törlése|Törli a meglévő átjáró példánya.|
|/Gateways/listLoginUris/Action|Jogkivonat-tároló tölti fel, és visszaadja az OAuth-bejelentkezés URI-azonosítók.|
|/Gateways/listKeys/Action|Átjáró titkok adja vissza.|
|/Gateways/Tokens/Write|Létrehoz egy új Zumo tokent hello megadott névvel.|
|/Gateways/Registrations/Read|Beolvasása hello hello átjáró regisztrált erőforrás (webhely).|
|/Gateways/Registrations/Write|Hello átjáró regisztrálja egy erőforrást (webhely).|
|/Gateways/Registrations/DELETE|Megszünteti a hello átjáró erőforrás (webhely).|
|/ apiapps/Olvasás|Beolvasása hello API-alkalmazáspéldányban.|
|/ apiapps/írása|Létrehoz egy új API-alkalmazást, vagy frissíti a meglévőt.|
|/ apiapps/törlése|Törli a meglévő API-alkalmazáspéldányban.|
|/apiapps/listStatus/Action|API-alkalmazás állapotának beolvasása.|
|/apiapps/listKeys/Action|API-alkalmazás titkos kulcsok adja vissza.|
|/apiapps/apidefinitions/Read|API-alkalmazás API-definíció adja vissza.|

## <a name="microsoftauthorization"></a>Microsoft.Authorization

| Művelet | Leírás |
|---|---|
|/ elevateAccess/művelet|Engedélyezi a hívó felhasználói hozzáférés adminisztrátora hozzáférést hello bérlői hatókörhöz hello|
|/classicAdministrators/Read|Hello rendszergazdák hello előfizetés beolvasása.|
|/ classicAdministrators/írása|Hozzáadása vagy módosítása a rendszergazda tooa előfizetés.|
|/classicAdministrators/DELETE|Hello rendszergazda eltávolítása hello előfizetésből.|
|/Locks/Read|Hello zárolásainak lekérdezi a megadott hatókör.|
|/ zárolások/írása|A megadott hello zárolásainak hatókör.|
|/Locks/DELETE|A megadott hatókör hello zárolásainak törlése.|
|/policyAssignments/Read|Szabályzat-hozzárendelés adatainak lekérése.|
|/ policyAssignments/írása|Hozzon létre egy házirend-hozzárendelés hello megadott hatókörben.|
|/policyAssignments/DELETE|Hello házirend-hozzárendelés törlése a megadott hatókörben.|
|/permissions/Read|Megjeleníti az összes hello engedélyeket hello hívó adott hatókörben van.|
|/roleDefinitions/Read|Szerepkör-definíció adatainak beolvasása.|
|/ roleDefinitions/írása|Egy egyéni szerepkör-definíció létrehozása vagy módosítása a megadott engedélyekkel és hozzárendelhető hatókörökkel.|
|/roleDefinitions/DELETE|Törölje a megadott egyéni szerepkör-definíció hello.|
|/providerOperations/Read|Az összes, szerepkör-definíciókban használható erőforrás-szolgáltató műveleteinek beolvasása.|
|/policyDefinitions/Read|Szabályzat-definíció adatainak lekérése.|
|/ policyDefinitions/írása|Egyéni házirend-definíció létrehozása.|
|/policyDefinitions/DELETE|Házirend-definíció törlése.|
|/roleAssignments/Read|Információk beolvasása a szerepkör-hozzárendelésről.|
|/ roleAssignments/írása|Hozzon létre egy szerepkör-hozzárendelés hello megadott hatókörben.|
|/roleAssignments/DELETE|Hello szerepkör-hozzárendelés törlése a megadott hatókörben.|

## <a name="microsoftautomation"></a>Microsoft.Automation

| Művelet | Leírás |
|---|---|
|/automationAccounts/Read|Egy Azure Automation-fiók beolvasása|
|/ automationAccounts/írása|Létrehozza vagy frissíti az Azure Automation-fiók|
|/automationAccounts/DELETE|Egy Azure Automation-fiók törlése|
|/automationAccounts/configurations/readContent/Action|Egy Azure Automation DSC tartalmának beolvasása|
|/automationAccounts/hybridRunbookWorkerGroups/Read|Beolvassa a hibrid forgatókönyv-feldolgozó erőforrások|
|/automationAccounts/hybridRunbookWorkerGroups/DELETE|Hibrid forgatókönyv-feldolgozó erőforrások törlése|
|/automationAccounts/jobSchedules/Read|Egy Azure Automation-feladatütemezés beolvasása|
|/automationAccounts/jobSchedules/Write|Egy Azure Automation-feladatütemezés létrehozása|
|/automationAccounts/jobSchedules/DELETE|Egy Azure Automation-feladatütemezés törlése|
|/automationAccounts/connectionTypes/Read|Egy Azure Automation szolgáltatásbeli kapcsolattípus-eszköz beolvasása|
|/automationAccounts/connectionTypes/Write|Egy Azure Automation szolgáltatásbeli kapcsolattípus-eszköz létrehozása|
|/automationAccounts/connectionTypes/DELETE|Egy Azure Automation szolgáltatásbeli kapcsolattípus-eszköz törlése|
|/automationAccounts/Modules/Read|Egy Azure Automation-modul beolvasása|
|/automationAccounts/Modules/Write|Létrehozza vagy frissíti az Azure Automation-modul|
|/automationAccounts/Modules/DELETE|Egy Azure Automation-modul törlése|
|/automationAccounts/credentials/Read|Egy Azure Automation szolgáltatásbeli hitelesítőadat-eszköz beolvasása|
|/automationAccounts/credentials/Write|Létrehozza vagy frissíti egy Azure Automation szolgáltatásbeli hitelesítőadat-eszköz|
|/automationAccounts/credentials/DELETE|Egy Azure Automation szolgáltatásbeli hitelesítőadat-eszköz törlése|
|/automationAccounts/Certificates/Read|Egy Azure Automation szolgáltatásbeli tanúsítványeszköz beolvasása|
|/automationAccounts/Certificates/Write|Létrehozza vagy frissíti az Azure Automation szolgáltatásbeli tanúsítványeszköz|
|/automationAccounts/Certificates/DELETE|Egy Azure Automation szolgáltatásbeli tanúsítványeszköz törlése|
|/automationAccounts/schedules/Read|Egy Azure Automation szolgáltatásbeli ütemezési eszköz beolvasása|
|/automationAccounts/schedules/Write|Létrehozza vagy frissíti az Azure Automation szolgáltatásbeli ütemezési eszköz|
|/automationAccounts/schedules/DELETE|Egy Azure Automation szolgáltatásbeli ütemezéseszköz törlése|
|/automationAccounts/Jobs/Read|Egy Azure Automation-feladat beolvasása|
|/automationAccounts/Jobs/Write|Egy Azure Automation-feladat létrehozása|
|/automationAccounts/Jobs/STOP/Action|Egy Azure Automation-feladat leállítása|
|/automationAccounts/Jobs/suspend/Action|Egy Azure Automation-feladat felfüggesztése|
|/automationAccounts/Jobs/Resume/Action|Egy Azure Automation-feladat folytatása|
|/automationAccounts/Jobs/runbookContent/Action|Hello feladat végrehajtási ideje hello hello hello Azure Automation-runbook tartalmának beolvasása|
|/automationAccounts/Jobs/output/Action|Hello feladat kimenetének beolvasása|
|/automationAccounts/Jobs/Read|Egy Azure Automation-feladat beolvasása|
|/automationAccounts/Jobs/Write|Egy Azure Automation-feladat létrehozása|
|/automationAccounts/Jobs/STOP/Action|Egy Azure Automation-feladat leállítása|
|/automationAccounts/Jobs/suspend/Action|Egy Azure Automation-feladat felfüggesztése|
|/automationAccounts/Jobs/Resume/Action|Egy Azure Automation-feladat folytatása|
|/automationAccounts/Jobs/STREAMS/Read|Egy Azure Automation-feladatstream beolvasása|
|/automationAccounts/Connections/Read|Azure Automation szolgáltatásbeli kapcsolódási eszköz beolvasása|
|/automationAccounts/Connections/Write|Létrehozza vagy frissíti az Azure Automation szolgáltatásbeli kapcsolódási eszköz|
|/automationAccounts/Connections/DELETE|Azure Automation szolgáltatásbeli kapcsolódási eszköz törlése|
|/automationAccounts/variables/Read|Egy Azure Automation szolgáltatásbeli változóeszköz beolvasása|
|/automationAccounts/variables/Write|Létrehozza vagy frissíti egy Azure Automation szolgáltatásbeli változóeszköz|
|/automationAccounts/variables/DELETE|Egy Azure Automation szolgáltatásbeli változóeszköz törlése|
|/automationAccounts/runbooks/readContent/Action|Hello egy Azure Automation-runbook tartalmának beolvasása|
|/automationAccounts/runbooks/Read|Egy Azure Automation-runbook beolvasása|
|/automationAccounts/runbooks/Write|Létrehozza vagy frissíti az Azure Automation-runbook|
|/automationAccounts/runbooks/DELETE|Egy Azure Automation-runbook törlése|
|/automationAccounts/runbooks/draft/readContent/Action|Hello egy Azure Automation-runbookvázlat tartalmának beolvasása|
|/automationAccounts/runbooks/draft/writeContent/Action|Hello egy Azure Automation-runbookvázlat tartalmának létrehozása|
|/automationAccounts/runbooks/draft/Read|Egy Azure Automation-runbookvázlat beolvasása|
|/automationAccounts/runbooks/draft/publish/Action|Egy Azure Automation-runbookvázlat közzététele|
|/automationAccounts/runbooks/draft/undoEdit/Action|Visszavonja a módosításokat tooan Azure Automation-runbookvázlat|
|/automationAccounts/runbooks/draft/testJob/Read|Egy Azure Automation runbookvázlat-tesztelési feladat beolvasása|
|/automationAccounts/runbooks/draft/testJob/Write|Egy Azure Automation runbookvázlat-tesztelési feladat létrehozása|
|/automationAccounts/runbooks/draft/testJob/STOP/Action|Egy Azure Automation runbookvázlat-tesztelési feladat leállítása|
|/automationAccounts/runbooks/draft/testJob/suspend/Action|Egy Azure Automation runbookvázlat-tesztelési feladat felfüggesztése|
|/automationAccounts/runbooks/draft/testJob/Resume/Action|Egy Azure Automation runbookvázlat-tesztelési feladat folytatása|
|/automationAccounts/webhooks/Read|Egy Azure Automation-webhook beolvasása|
|/automationAccounts/webhooks/Write|Létrehozza vagy frissíti az Azure Automation-webhook|
|/automationAccounts/webhooks/DELETE|Egy Azure Automation-webhook törlése |
|/automationAccounts/webhooks/generateUri/Action|URI generálása Azure Automation-webhook|

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

A szolgáltató nem egy teljes ARM-szolgáltató, és nem biztosít semmilyen ARM műveletek.

## <a name="microsoftbatch"></a>Microsoft.Batch

| Művelet | Leírás |
|---|---|
|/ regisztrációs/művelet|Hello kötegelt erőforrás-szolgáltató hello előfizetést regisztrál, és lehetővé teszi a Batch-fiókok létrehozását hello|
|/ batchAccounts/írása|Új Batch-fiók létrehozása vagy frissítése egy meglévő Batch-fiók|
|/batchAccounts/Read|Batch-fiókok listája, vagy a Batch-fiók hello tulajdonságainak beolvasása|
|/batchAccounts/DELETE|Batch-fiók törlése|
|/batchAccounts/listkeys/Action|Listák hívóbetűk Batch-fiókhoz|
|/batchAccounts/regeneratekeys/Action|A Batch-fiókhoz hívóbetűk újragenerálása|
|/batchAccounts/syncAutoStorageKeys/Action|Szinkronizálja a Batch-fiókhoz konfigurált hello automatikus tárfiók hozzáférési kulcsainak listázása|
|/batchAccounts/Applications/Read|Alkalmazások listája vagy az alkalmazás hello tulajdonságainak beolvasása|
|/batchAccounts/Applications/Write|Létrehoz egy új alkalmazást, vagy egy meglévő alkalmazás frissítése|
|/batchAccounts/Applications/DELETE|Az alkalmazás törlése|
|/batchAccounts/Applications/versions/Read|Alkalmazáscsomag hello tulajdonságainak beolvasása|
|/batchAccounts/Applications/versions/Write|Létrehoz egy új alkalmazás-csomagot, vagy frissíti a meglévő alkalmazáscsomag|
|/batchAccounts/Applications/versions/Activate/Action|Az alkalmazáscsomag aktiválása|
|/batchAccounts/Applications/versions/DELETE|Az alkalmazáscsomag törlése|
|/Locations/Quotas/Read|Lekérdezi kötegelt kvótái hello megadott előfizetést hello megadott Azure-régió|

## <a name="microsoftbilling"></a>Microsoft.Billing

| Művelet | Leírás |
|---|---|
|/Invoices/Read|Rendelkezésre álló listák számlák|

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

| Művelet | Leírás |
|---|---|
|/ mapApis/Olvasás|Olvasási művelete|
|/ mapApis/írása|Írási művelet|
|/ mapApis/törlése|Törlési művelet|
|/mapApis/regenerateKey/Action|Hello kulcs újragenerálása|
|/mapApis/listSecrets/Action|Lista hello titkos kulcsok|
|/mapApis/listSingleSignOnToken/Action|Olvasási egyszeri bejelentkezési a engedélyezési jogkivonat a hello erőforrás|
|Műveletek/olvasása|Hello művelet leírása.|

## <a name="microsoftcache"></a>Microsoft.Cache

| Művelet | Leírás |
|---|---|
|/ checknameavailability/művelet|Ellenőrzi, hogy a név egy új Redis Cache használható|
|/ regisztrációs/művelet|Az előfizetéshez hello "Microsoft.Cache" erőforrás-szolgáltató regisztrálása|
|/ unregister/művelet|Hello "Microsoft.Cache" erőforrás-szolgáltató előfizetés regisztrációjának törlése|
|/ redis/írása|Módosítsa a hello Redis gyorsítótár beállításainak és konfigurációjának hello felügyeleti portálon|
|/redis/Read|Hello Redis gyorsítótár beállításainak és konfigurációjának megtekintése hello felügyeleti portálon|
|/redis/DELETE|Törölje a teljes Redis Cache hello|
|/redis/listKeys/Action|Redis gyorsítótár elérésikulcs hello kezelési portál hello értékének megtekintése|
|/redis/regenerateKey/Action|Redis gyorsítótár elérési kulcsainak hello kezelési portál hello értékének módosítása|
|/redis/import/Action|Meghatározott formátumú adatok importálása a Redis szolgáltatásba több blobból|
|/redis/export/Action|Exportálja a Redis tooprefixed tárolási BLOB-adatobjektumok megadott formátumban|
|/redis/forceReboot/Action|Egy gyorsítótárpéldány kényszerített újraindítása, mely adatvesztést okozhat.|
|/redis/STOP/Action|Állítsa le a gyorsítótárpéldány.|
|/redis/Start/Action|Indítsa el a gyorsítótárpéldány.|
|/redis/metricDefinitions/Read|Az egy Redis gyorsítótárhoz elérhető metrikai hello beolvasása|
|/redis/firewallRules/Read|Egy Redis gyorsítótárhoz hello IP tűzfalszabályt beolvasása|
|/redis/firewallRules/Write|Egy Redis gyorsítótárhoz hello IP tűzfalszabályt szerkesztése|
|/redis/firewallRules/DELETE|Egy Redis gyorsítótárhoz IP tűzfalszabályt törlése|
|/redis/listUpgradeNotifications/Read|Lista hello hello gyorsítótár bérlője legutóbbi Csomagváltási értesítések.|
|/redis/linkedservers/Read|A redis gyorsítótár társított csatolt kiszolgálók beolvasása.|
|/redis/linkedservers/Write|A csatolt kiszolgáló tooa Redis Cache hozzáadása|
|/redis/linkedservers/DELETE|A csatolt kiszolgáló törlése a Redis gyorsítótár|
|/redis/patchSchedules/Read|Lekérdezi a hello javítását a Redis Cache ütemezését|
|/redis/patchSchedules/Write|Egy Redis gyorsítótárhoz ütemezését javítását hello módosítása|
|/redis/patchSchedules/DELETE|Egy Redis gyorsítótárhoz hello javítás ütemezés törlése|

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

| Művelet | Leírás |
|---|---|
|/ provisionGlobalAppServicePrincipalInUserTenant/művelet|Az alkalmazás egyszerű kiépítés egyszerű szolgáltatásnév|
|/ validateCertificateRegistrationInformation/művelet|Ellenőrizze a beszerzési tanúsítványobjektum lefolytatása nélkül|
|/ regisztrációs/művelet|Hello Microsoft Certificates erőforrás-szolgáltató hello előfizetés regisztrálása|
|/ certificateOrders/írása|Adja hozzá egy új certificateOrder vagy egy meglévő frissítése|
|/ certificateOrders/törlése|Egy meglévő AppServiceCertificate törlése|
|/ certificateOrders/Olvasás|Itt olvashatók CertificateOrders hello listája|
|/certificateOrders/reissue/Action|Hajtsa végre újból egy meglévő certificateorder|
|/certificateOrders/renew/Action|Egy meglévő certificateorder megújítása|
|/certificateOrders/retrieveCertificateActions/Action|Tanúsítvány műveletek hello listájának beolvasása|
|/certificateOrders/retrieveEmailHistory/Action|Tanúsítvány e-mail előzmények beolvasása|
|/certificateOrders/resendEmail/Action|Tanúsítvány e-mail újraküldése|
|/certificateOrders/verifyDomainOwnership/Action|Ellenőrizze a tartomány tulajdonosa|
|/certificateOrders/resendRequestEmails/Action|Küldje el újra a kérelmet e-mailek tooanother e-mail cím|
|/certificateOrders/resendRequestEmails/Action|App Service kiállított tanúsítvány hely lezárása beolvasása|
|/certificateOrders/Certificates/Write|Adja hozzá az új tanúsítványt, vagy egy meglévő frissítése|
|/certificateOrders/Certificates/DELETE|Meglévő tanúsítvány törlése|
|/certificateOrders/Certificates/Read|Tanúsítványok hello listájának beolvasása|

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

| Művelet | Leírás |
|---|---|
|/ regisztrációs/művelet|Számítási tooClassic regisztrálása|
|/ checkDomainNameAvailability/művelet|Az adott tartománynév hello rendelkezésre állását ellenőrzi.|
|/ moveSubscriptionResources/művelet|Helyezze át az összes hagyományos erőforrás tooa másik előfizetést.|
|/ validateSubscriptionMoveAvailability/művelet|Klasszikus áthelyezési művelet hello előfizetés rendelkezésre állásának ellenőrzése.|
|/operatingSystemFamilies/Read|Az hello Vendég operációsrendszer-család a Microsoft Azure-ban elérhető, illetve felsorolja is hello operációs rendszereken érhető el minden f
|/Capabilities/Read|Hello képességeit jeleníti meg|
|/operatingSystems/Read|Felsorolja a hello hello vendég operációs rendszer azon verzióit, amelyek jelenleg a Microsoft Azure-ban.|
|/resourceTypes/skus/Read|Támogatott erőforrástípusai hello Sku lista beolvasása.|
|/domainNames/Read|Erőforrások tartománynevének hello visszaadása.|
|/ domainNames/írása|Hozzáadása vagy módosítása erőforrások tartománynevének hello.|
|/domainNames/DELETE|Erőforrások tartománynevének hello eltávolítása.|
|/domainNames/swap/Action|Átmeneti tárolóhely toohello éles tárolóhelyre hello cseréje.|
|/domainNames/serviceCertificates/Read|Vissza az alkalmazott szolgáltatási tanúsítványok hello.|
|/domainNames/serviceCertificates/Write|Hozzáadása vagy módosítása az alkalmazott szolgáltatási tanúsítványok hello.|
|/domainNames/serviceCertificates/DELETE|Az alkalmazott szolgáltatási tanúsítványok hello törlése.|
|/domainNames/serviceCertificates/operationStatuses/Read|Hello hello tartomány tartománynevek szolgáltatási tanúsítványai műveleti állapotának beolvasása.|
|/domainNames/Capabilities/Read|Megjeleníti a hello tartományt képességek|
|/domainNames/Extensions/Read|Vissza a tartománynév-kiterjesztések hello.|
|/domainNames/Extensions/Write|Hello tartománynév-kiterjesztések hozzáadása.|
|/domainNames/Extensions/DELETE|Hello tartománynév-kiterjesztések eltávolítása.|
|/domainNames/Extensions/operationStatuses/Read|Hello hello tartomány kiterjesztések műveleti állapotának beolvasása.|
|/domainNames/Active/Write|Hello aktív tartománynév beállítása.|
|/domainNames/slots/Read|Hello üzembe helyezési jeleníti meg.|
|/domainNames/slots/Write|Létrehozza vagy hello üzemelő példány frissítése.|
|/domainNames/slots/DELETE|A megadott üzembe helyezési pont törlése.|
|/domainNames/slots/Start/Action|Üzembe helyezési pont indítása.|
|/domainNames/slots/STOP/Action|Felfüggeszti a hello üzembe helyezési pont.|
|/domainNames/slots/operationStatuses/Read|Hello hello tartomány tartománynevek üzembe helyezési pontjai műveleti állapotának beolvasása.|
|/domainNames/slots/Roles/Read|Hello szerepkör lekérése hello üzembe helyezési pont.|
|/domainNames/slots/Roles/extensionReferences/Read|Beolvasása hello hello üzembe helyezési ponti szerepkör kiterjesztéshivatkozását.|
|/domainNames/slots/Roles/extensionReferences/Write|Hozzáadása vagy módosítása hello hello üzembe helyezési ponti szerepkör kiterjesztéshivatkozását.|
|/domainNames/slots/Roles/extensionReferences/DELETE|Távolítsa el a hello hello üzembe helyezési ponti szerepkör kiterjesztéshivatkozását.|
|/domainNames/slots/Roles/extensionReferences/operationStatuses/Read|Hello hello tartomány nevek üzembe helyezési ponti szerepkörei kiterjesztéshivatkozásai műveleti állapotának beolvasása.|
|/domainNames/slots/Roles/roleInstances/Read|Hello szerepkörpéldányt beolvasása.|
|/domainNames/slots/Roles/roleInstances/restart/Action|Újraindítja a szerepkörpéldányt beállítani.|
|/domainNames/slots/Roles/roleInstances/reimage/Action|Reimages hello szerepkörpéldányt.|
|/domainNames/slots/Roles/roleInstances/operationStatuses/Read|Hello hello tartomány nevek üzembe helyezési ponti szerepkörei szerepkörpéldányai műveleti állapotának beolvasása.|
|/domainNames/slots/State/Start/Write|Módosítások hello telepítési tárolóhely állapotát toostopped.|
|/domainNames/slots/State/STOP/Write|Módosítások hello telepítési tárolóhely állapotát toostarted.|
|/domainNames/slots/upgradeDomain/Write|Frissítési hello tartomány ismerteti.|
|/domainNames/internalLoadBalancers/Read|Hello belső terheléselosztók beolvasása.|
|/domainNames/internalLoadBalancers/Write|Új belső terheléselosztás létrehozása.|
|/domainNames/internalLoadBalancers/DELETE|Új belső terheléselosztás eltávolítása.|
|/domainNames/internalLoadBalancers/operationStatuses/Read|Hello műveleti állapotának beolvasása a hello tartománynevek belső terheléselosztóinak.|
|/domainNames/loadBalancedEndpointSets/Read|Megjeleníti a hello terhelésű végpontcsoportok megjelenítése|
|/domainNames/loadBalancedEndpointSets/operationStatuses/Read|Hello műveleti állapotának beolvasása a hello tartománynevek elosztott terhelésű végpontcsoportok beolvasása.|
|/domainNames/availabilitySets/Read|Hello rendelkezésre állási készlet hello erőforrás megjelenítése.|
|/Quotas/Read|Hello előfizetés hello kvótájának beolvasása.|
|/virtualMachines/Read|Virtuális gépek listájának beolvasása.|
|/ virtuális gépek vannak/írása|Virtuális gépek hozzáadása vagy módosítása.|
|/virtualMachines/DELETE|Eltávolítja a virtuális gépek.|
|/virtualMachines/Start/Action|Hello virtuális gép elindításához.|
|/virtualMachines/redeploy/Action|Redeploys hello virtuális gépet.|
|/virtualMachines/restart/Action|Újraindítja a virtuális gépek.|
|/virtualMachines/STOP/Action|Leállítja a virtuális gép hello.|
|/virtualMachines/shutdown/Action|Leállítás hello virtuális gépet.|
|/virtualMachines/attachDisk/Action|Csatolja az adatok lemezre tooa virtuális gép.|
|/virtualMachines/detachDisk/Action|Adatlemez leválasztása a virtuális gépről.|
|/virtualMachines/downloadRemoteDesktopConnectionFile/Action|A virtuális gép RDP-fájl hello tölti le.|
|/virtualMachines/hálózati illesztők /<br>associatedNetworkSecurityGroups olvasása|Hello hello hálózati interfészhez társított hálózati biztonsági csoport lekérése.|
|/virtualMachines/hálózati illesztők /<br>associatedNetworkSecurityGroups írása|Hello hálózati interfészhez társított hálózati biztonsági csoport hozzáadása.|
|/virtualMachines/hálózati illesztők /<br>associatedNetworkSecurityGroups vagy törlése|Hello hello hálózati interfészhez társított hálózati biztonsági csoport törlése.|
|/virtualMachines/hálózati illesztők /<br>olvasási idő associatedNetworkSecurityGroups/operationStatuses|Hello műveleti állapotának hello virtuális gépek társított hálózati biztonsági csoportok beolvasása.|
|/virtualMachines/Providers/Microsoft.Insights/metricDefinitions/Read|Hello metrikák meghatározások beolvasása.|
|/virtualMachines/Providers/Microsoft.Insights/diagnosticSettings/Read|Hello diagnosztikai beállításainak beolvasása.|
|/virtualMachines/Providers/Microsoft.Insights/diagnosticSettings/Write|Diagnosztikai beállítások hozzáadása vagy módosítása.|
|/virtualMachines/Metrics/Read|Hello metrikák beolvasása.|
|/virtualMachines/operationStatuses/Read|Hello hello virtuális gépek műveleti állapotának beolvasása.|
|/virtualMachines/Extensions/Read|Hello virtuálisgép-bővítmény beolvasása.|
|/virtualMachines/Extensions/Write|Virtuálisgép-bővítmény hello helyezi.|
|/virtualMachines/Extensions/operationStatuses/Read|Hello hello virtuálisgép-bővítmények műveleti állapotának beolvasása.|
|/virtualMachines/asyncOperations/Read|Hello lehetséges aszinkron műveletek beolvasása|
|/virtualMachines/Disks/Read|Adatlemezek listájának beolvasása|
|/virtualMachines/associatedNetworkSecurityGroups/Read|Hello hello virtuális gép társított hálózati biztonsági csoport lekérése.|
|/virtualMachines/associatedNetworkSecurityGroups/Write|Hello virtuális géphez társított hálózati biztonsági csoport hozzáadása.|
|/virtualMachines/associatedNetworkSecurityGroups/DELETE|Hello hello virtuális gép társított hálózati biztonsági csoport törlése.|
|/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/Read|Hello műveleti állapotának hello virtuális gépek társított hálózati biztonsági csoportok beolvasása.|

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

| Művelet | Leírás |
|---|---|
|/ regisztrációs/művelet|Hálózati tooClassic regisztrálása|
|/gatewaySupportedDevices/Read|Támogatott eszközök hello listájának beolvasása.|
|/reservedIps/Read|Lekérdezi hello foglalt IP-cím|
|/ Keskenyeknek/írása|Új fenntartott IP-cím felvétele|
|/reservedIps/DELETE|Fenntartott IP-cím törlése.|
|/reservedIps/Link/Action|Egy fenntartott IP-cím hivatkozása|
|/reservedIps/JOIN/Action|Csatlakozás egy fenntartott IP-címhez|
|/reservedIps/operationStatuses/Read|Hello hello szolgáltatás számára fenntartott IP-címek műveleti állapotának beolvasása.|
|/virtualNetworks/Read|Virtuális hálózati hello beolvasása.|
|/ virtualNetworks/írása|Új virtuális hálózat hozzáadása.|
|/virtualNetworks/DELETE|Hello virtuális hálózat törlése.|
|/virtualNetworks/Peer/Action|Társviszony-létesítés két virtuális hálózat között.|
|/virtualNetworks/JOIN/Action|Hello virtuális hálózathoz csatlakozik.|
|/virtualNetworks/checkIPAddressAvailability/Action|Egy adott IP-cím része virtuális hálózatnak hello rendelkezésre állását ellenőrzi.|
|/virtualNetworks/Capabilities/Read|Hello képességeit jeleníti meg|
|/virtualNetworks/alhálózatok /<br>associatedNetworkSecurityGroups olvasása|Hello hello alhálózathoz társított hálózati biztonsági csoport lekérése.|
|/virtualNetworks/alhálózatok /<br>associatedNetworkSecurityGroups írása|Hello alhálózathoz társított hálózati biztonsági csoport hozzáadása.|
|/virtualNetworks/alhálózatok /<br>associatedNetworkSecurityGroups vagy törlése|Hello hello alhálózathoz társított hálózati biztonsági csoport törlése.|
|/virtualNetworks/alhálózatok /<br>olvasási idő associatedNetworkSecurityGroups/operationStatuses|Hello hello virtuális hálózat alhálózatához társított hálózati biztonsági csoport műveleti állapotának beolvasása.|
|/virtualNetworks/operationStatuses/Read|Hello hello virtuális hálózatok műveleti állapotának beolvasása.|
|/virtualNetworks/Gateways/Read|Hello virtuális hálózati átjárók beolvasása.|
|/virtualNetworks/Gateways/Write|Virtuális hálózati átjáró hozzáadása.|
|/virtualNetworks/Gateways/DELETE|Hello virtuális hálózati átjáró törlése.|
|/virtualNetworks/Gateways/startDiagnostics/Action|Hello virtuális hálózati átjáró diagnosztikájának indítása.|
|/virtualNetworks/Gateways/stopDiagnostics/Action|Leállítja a hello virtuális hálózati átjáró diagnosztikájának hello.|
|/virtualNetworks/Gateways/downloadDiagnostics/Action|Hello átjáródiagnosztikai tölti le.|
|/virtualNetworks/Gateways/listCircuitServiceKey/Action|Hello kör szolgáltatáskulcsának beolvasása.|
|/virtualNetworks/Gateways/downloadDeviceConfigurationScript/Action|Hello eszközkonfigurációs parancsprogram tölti le.|
|/virtualNetworks/Gateways/listPackage/Action|Virtuális hálózati átjárócsomag hello sorolja fel.|
|/virtualNetworks/Gateways/operationStatuses/Read|Hello hello virtuális hálózati átjárók műveleti állapotának beolvasása.|
|/virtualNetworks/Gateways/Packages/Read|Hello virtuális hálózati átjárócsomag beolvasása.|
|/virtualNetworks/Gateways/Connections/Read|Kapcsolatok hello listájának beolvasása.|
|/virtualNetworks/Gateways/Connections/Connect/Action|A kapcsolatot egy hely toosite gateway-kapcsolatot.|
|/virtualNetworks/Gateways/Connections/disconnect/Action|Egy hely toosite átjárókapcsolat leválasztása.|
|/virtualNetworks/Gateways/Connections/test/Action|Egy hely toosite gateway-kapcsolatot ellenőrzi.|
|/virtualNetworks/Gateways/clientRevokedCertificates/Read|Olvasási hello visszavont ügyféltanúsítványok.|
|/virtualNetworks/Gateways/clientRevokedCertificates/Write|Ügyféltanúsítvány visszavonása.|
|/virtualNetworks/Gateways/clientRevokedCertificates/DELETE|Az ügyféltanúsítvány visszavonásának megszüntetése.|
|/virtualNetworks/Gateways/clientRootCertificates/Read|Hello ügyfél legfelső szintű tanúsítványok keresése.|
|/virtualNetworks/Gateways/clientRootCertificates/Write|Ügyfél új főtanúsítványának feltöltése.|
|/virtualNetworks/Gateways/clientRootCertificates/DELETE|Hello virtuális hálózati átjáró ügyféltanúsítványának törlése.|
|/virtualNetworks/Gateways/clientRootCertificates/download/Action|Tanúsítvány letöltése ujjlenyomat útján.|
|/virtualNetworks/Gateways/clientRootCertificates/listPackage/Action|Hello virtuális hálózati átjárócsomag tanúsítvány sorolja fel.|
|/networkSecurityGroups/Read|Hello hálózati biztonsági csoport lekérése.|
|/ biztonsági csoportok/írása|Új hálózati biztonsági csoport hozzáadása.|
|/networkSecurityGroups/DELETE|Hello hálózati biztonsági csoport törlése.|
|/networkSecurityGroups/operationStatuses/Read|Hello hello hálózati biztonsági csoport műveleti állapotának beolvasása.|
|/networkSecurityGroups/securityRules/Read|Hello biztonsági szabály beolvasása.|
|/networkSecurityGroups/securityRules/Write|Egy biztonsági szabály hozzáadása vagy frissítése.|
|/networkSecurityGroups/securityRules/DELETE|Hello biztonsági szabály törlése.|
|/networkSecurityGroups/securityRules/operationStatuses/Read|Hello hello hálózati biztonsági csoport biztonsági szabályai műveleti állapotának beolvasása.|
|/Quotas/Read|Hello előfizetés hello kvótájának beolvasása.|

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

| Művelet | Leírás |
|---|---|
|/ regisztrációs/művelet|Tárolási tooClassic regisztrálása|
|/ checkStorageAccountAvailability/művelet|A tárfiók hello rendelkezésre állását ellenőrzi.|
|/Capabilities/Read|Hello képességeit jeleníti meg|
|/publicImages/Read|Lekérdezi a hello nyilvános virtuálisgép-lemezkép.|
|/Images/Read|Hello kép értéket ad vissza.|
|/storageAccounts/Read|Hello tárfiók visszaadása a megadott fiók hello.|
|/ storageAccounts/írása|Új tárfiók hozzáadása.|
|/storageAccounts/DELETE|Hello a tárfiók törlése.|
|/storageAccounts/listKeys/Action|Hello tárfiókok elérési kulcsainak hello sorolja fel.|
|/storageAccounts/regenerateKey/Action|Hello tárfiók hello meglévő elérési kulcsainak újragenerálása.|
|/storageAccounts/operationStatuses/Read|Hello erőforrás hello műveleti állapotának beolvasása.|
|/storageAccounts/Images/Read|Vissza a tárfióklemezkép hello.|
|/storageAccounts/Images/DELETE|A megadott tárfióklemezkép törlése.|
|/storageAccounts/Disks/Read|Értéket ad vissza a fiók tárolólemez hello.|
|/storageAccounts/Disks/Write|Új tárfióklemez felvétele.|
|/storageAccounts/Disks/DELETE|A megadott tárfióklemez törlése.|
|/storageAccounts/Disks/operationStatuses/Read|Hello erőforrás hello műveleti állapotának beolvasása.|
|/storageAccounts/osImages/Read|Vissza a tárfiók operációs rendszer lemezképének hello.|
|/storageAccounts/osImages/DELETE|A megadott tárfiók operációsrendszer-lemezképének törlése.|
|/storageAccounts/Services/Read|Első hello elérhető szolgáltatások.|
|/storageAccounts/Services/metricDefinitions/Read|Hello metrikák meghatározások beolvasása.|
|/storageAccounts/Services/Metrics/Read|Hello metrikák beolvasása.|
|/storageAccounts/Services/diagnosticSettings/Read|Hello diagnosztikai beállításainak beolvasása.|
|/storageAccounts/Services/diagnosticSettings/Write|Diagnosztikai beállítások hozzáadása vagy módosítása.|
|/Disks/Read|Értéket ad vissza a fiók tárolólemez hello.|
|/osImages/Read|Hello operációs rendszer lemezképének értéket ad vissza.|
|/Quotas/Read|Hello előfizetés hello kvótájának beolvasása.|

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

| Művelet | Leírás |
|---|---|
|/accounts/Read|Olvassa be az API-fiókokat.|
|/ fiókok/írása|Írja az API-fiókokat.|
|/accounts/DELETE|Törli az API-fiókok|
|/accounts/listKeys/Action|Listázása|
|/accounts/regenerateKey/Action|Kulcs újragenerálása|
|/accounts/skus/Read|Elérhető termékváltozatok a meglévő erőforrás beolvasása.|
|/accounts/usages/Read|Hello kvótahasználat lekérése a meglévő erőforrás.|
|Műveletek/olvasása|Hello művelet leírása.|

## <a name="microsoftcommerce"></a>Microsoft.Commerce

| Művelet | Leírás |
|---|---|
|RateCard/olvasása|Értéket ad vissza adatokat, az Erőforrás-kijelző metaadatok és a megadott előfizetés hello díjakat kínál.|
|UsageAggregates/olvasása|Lekéri a Microsoft Azure használat előfizetés. hello eredmény összesítések tartalmaz a kapcsolódó információk, egy adott időtartományt a használati adatok, az előfizetés és az erőforrás.|

## <a name="microsoftcompute"></a>Microsoft.Compute

| Művelet | Leírás |
|---|---|
|/ regisztrációs/művelet|Az előfizetés regisztrálása a Microsoft.Compute erőforrás-szolgáltatónál|
|/restorePointCollections/Read|A visszaállítási pont gyűjtemény hello tulajdonságainak beolvasása|
|/ restorePointCollections/írása|Létrehoz egy új helyreállítási pont gyűjteményt vagy frissít egy meglévő|
|/restorePointCollections/DELETE|Törlések hello visszaállítási pont adatgyűjtési és -tárolt visszaállítási pontok|
|/restorePointCollections/restorePoints/Read|Visszaállítási pont hello tulajdonságainak beolvasása|
|/restorePointCollections/restorePoints/Write|Új helyreállítási pont létrehozása|
|/restorePointCollections/restorePoints/DELETE|Törli a hello visszaállítási pont|
|/restorePointCollections/restorePoints/retrieveSasUris/Action|SAS URI blob együtt visszaállítási pont hello tulajdonságainak beolvasása|
|/virtualMachineScaleSets/Read|A virtuálisgép-méretezési csoport hello tulajdonságainak beolvasása|
|/ virtualMachineScaleSets/írása|Létrehoz egy új vagy frissít egy meglévő virtuálisgép-méretezési csoportot|
|/virtualMachineScaleSets/DELETE|Hello virtuálisgép-méretezési csoport törlése|
|/virtualMachineScaleSets/Start/Action|Elindul hello hello virtuálisgép-méretezési csoport példányait|
|/virtualMachineScaleSets/powerOff/Action|Kikapcsolja a hello hello virtuálisgép-méretezési csoport példányait|
|/virtualMachineScaleSets/restart/Action|Hello példányok hello virtuálisgép-méretezési csoport újraindítása|
|/virtualMachineScaleSets/deallocate/Action|Kikapcsolja és kiadások hello számítási erőforrásokat hello hello virtuálisgép-méretezési csoport példányait |
|/virtualMachineScaleSets/manualUpgrade/Action|Frissíti a példányok toolatest modell hello virtuálisgép-méretezési csoport|
|/virtualMachineScaleSets/Scale/Action|A méretezési / horizontális Felskálázás példányszámot, egy meglévő virtuálisgép-méretezési beállítása|
|/virtualMachineScaleSets/instanceView/Read|Hello hello virtuálisgép-méretezési csoport példányait tartalmazó nézet beolvasása|
|/virtualMachineScaleSets/skus/Read|Listák hello egy meglévő virtuálisgép-méretezési csoport érvényes Termékváltozatait|
|/virtualMachineScaleSets/virtualMachines/Read|Lekéri a Virtuálisgép-méretezési csoportban lévő virtuális gép hello tulajdonságait|
|/virtualMachineScaleSets/virtualMachines/DELETE|Töröl egy virtuális gépet egy virtuálisgép-méretezési csoportban.|
|/virtualMachineScaleSets/virtualMachines/Start/Action|Elindít egy virtuálisgép-méretezési csoportban lévő virtuálisgép-példányt.|
|/virtualMachineScaleSets/virtualMachines/powerOff/Action|Kikapcsol egy virtuálisgép-példányt egy virtuálisgép-méretezési csoportban.|
|/virtualMachineScaleSets/virtualMachines/restart/Action|Újraindít egy virtuálisgép-méretezési csoportban lévő virtuálisgép-példányt.|
|/virtualMachineScaleSets/virtualMachines/deallocate/Action|Kikapcsolja és kiadások hello számítási erőforrások a Virtuálisgép-méretezési csoportban lévő virtuális gép.|
|/virtualMachineScaleSets/virtualMachines/instanceView/Read|Egy Virtuálisgép-méretezési csoportban lévő virtuális gép hello példányait tartalmazó nézet beolvasása.|
|/Images/Read|Hello kép hello tulajdonságainak beolvasása|
|/ képek/írása|Lemezképet hoz létre új vagy frissít egy meglévő|
|/Images/DELETE|Hello lemezkép törlése|
|/Operations/Read|A Microsoft.Compute erőforrás-szolgáltató elérhető műveleteinek listázása|
|/Disks/Read|A lemez hello tulajdonságainak beolvasása|
|/ lemezek írása|Új lemez létrehozása vagy meglévő lemez frissítése|
|/Disks/DELETE|Törlések hello lemez|
|/Disks/beginGetAccess/Action|Hello hello lemez SAS URI-t beolvasása blobhozzáféréshez|
|/Disks/endGetAccess/Action|Hello hello lemez SAS URI-t visszavonása|
|/snapshots/Read|Hello pillanatkép tulajdonságainak beolvasása|
|/ pillanatképek/írása|Új pillanatkép létrehozása vagy meglévő pillanatkép frissítése|
|/snapshots/DELETE|A pillanatkép törlése|
|/availabilitySets/Read|Rendelkezésre állási csoportok hello tulajdonságainak beolvasása|
|/ availabilitySets/írása|Létrehoz egy új vagy frissít egy meglévő rendelkezésre állási csoportot|
|/availabilitySets/DELETE|Hello rendelkezésre állási csoport törlése|
|/availabilitySets/vmSizes/Read|Létrehozásához vagy a virtuális gép rendelkezésre állási csoport hello frissítéséhez használható méretek listázása|
|/virtualMachines/Read|A virtuális gép hello tulajdonságainak beolvasása|
|/ virtuális gépek vannak/írása|Létrehoz egy új virtuális gépet vagy frissít egy meglévő virtuális gépet|
|/virtualMachines/DELETE|Hello virtuális gép törlése|
|/virtualMachines/Start/Action|Hello-virtuális gép indítása|
|/virtualMachines/powerOff/Action|Kikapcsolja a virtuális gép hello. Vegye figyelembe, hogy a virtuális gép hello számlázva toobe folytatódik.|
|/virtualMachines/redeploy/Action|Virtuális gép redeploys|
|/virtualMachines/restart/Action|Hello virtuális gép újraindul|
|/virtualMachines/deallocate/Action|Kikapcsolja a hello virtuális gépet és felszabadítja hello számítási erőforrásokat|
|/virtualMachines/generalize/Action|Hello virtuális gép állapota tooGeneralized beállítja és előkészíti a hello virtuális gépet a rögzítéshez|
|/virtualMachines/Capture/Action|Hello virtuális gép rögzíti a virtuális merevlemezek másolásával, és létrehoz egy sablont, amely használt toocreate hasonló virtuális gépek|
|/virtualMachines/convertToManagedDisks/Action|Hello blobalapú lemezek hello virtuális gép toomanaged lemezek konvertálása|
|/virtualMachines/vmSizes/Read|Megjeleníti az elérhető méretek hello virtuális gép frissíthető|
|/virtualMachines/instanceView/Read|Lekérdezi hello hello virtuális gép részletes futási állapotát és az erőforrások|
|/virtualMachines/Extensions/Read|A virtuálisgép-bővítmény hello tulajdonságainak beolvasása|
|/virtualMachines/Extensions/Write|Létrehoz egy új vagy frissít egy meglévő virtuálisgép-bővítményt|
|/virtualMachines/Extensions/DELETE|Hello virtuálisgép-bővítmény törlése|
|/Locations/vmSizes/Read|Listázza az adott helyen elérhető virtuálisgép-méreteket|
|/Locations/usages/Read|Szolgáltatások korlátait és az aktuális felhasználási mennyiségeket olvassa hello előfizetés számítási erőforrások lekérdezi a helyen|
|/Locations/Operations/Read|Egy aszinkron művelet hello állapotának beolvasása|

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

| Művelet | Leírás |
|---|---|
|/ regisztrációs/művelet|Hello előfizetés hello tároló beállításjegyzék erőforrás-szolgáltató regisztrálása, és lehetővé teszi a tároló nyilvántartó hello létrehozását.|
|/checknameavailability/Read|Ellenőrzi, ugyanez a neve érvényes, és nincs használatban.|
|/registries/Read|Beolvasása hello tároló nyilvántartó listája vagy lekérdezi hello hello megadott tároló beállításjegyzék tulajdonságait.|
|/ nyilvántartó/írása|Létrehoz egy tároló beállításjegyzéket hello megadott paramétereket, vagy hello tulajdonságok vagy címkék hello megadott tároló beállításjegyzék frissítése.|
|/registries/DELETE|Egy meglévő tároló beállításjegyzék törli.|
|/registries/listCredentials/Action|Hello megadott tároló beállításjegyzék hello bejelentkezési hitelesítő adatokat tartalmazza.|
|/registries/regenerateCredential/Action|Újragenerálja hello megadott tároló beállításjegyzék hello bejelentkezési hitelesítő adatokat.|

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

| Művelet | Leírás |
|---|---|
|/containerServices/Subscriptions/Read|Get hello megadott tárolószolgáltatások előfizetés alapján|
|/containerServices/resourceGroups/Read|Get hello megadott tárolószolgáltatások erőforráscsoport alapján|
|/containerServices/resourceGroups/ContainerServiceName/Read|A megadott Tárolószolgáltatás lekérdezi hello|
|/containerServices/resourceGroups/ContainerServiceName/Write|A megadott Tárolószolgáltatás visszahelyezi vagy frissítések hello|
|/containerServices/resourceGroups/ContainerServiceName/DELETE|A megadott Tárolószolgáltatás törlése hello|

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

| Művelet | Leírás |
|---|---|
|/ updateCommunicationPreference/művelet|Update kommunikációs beállítás|
|/ listCommunicationPreference/művelet|Lista kommunikációs beállításokat|
|/Applications/Read|Olvasási művelete|
|/ applications/írása|Írási művelet|
|/ applications/írása|Írási művelet|
|/Applications/DELETE|Törlési művelet|
|/Applications/listSecrets/Action|Titkos kulcsainak listázása|
|/Applications/listSingleSignOnToken/Action|Olvassa el a jogkivonatok az egyszeri bejelentkezés|
|/Operations/Read|olvasási műveletek|

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

| Művelet | Leírás |
|---|---|
|/hubs/Read|Bármely Azure felhasználói Insights Hub olvasása|
|/ hubok/írása|Hozzon létre vagy bármely Azure felhasználói Insights központ frissítése|
|/hubs/DELETE|Bármely Azure felhasználói Insights Hub törlése|
|/hubs/Providers/Microsoft.Insights/metricDefinitions/Read|Az erőforrás hello elérhető metrikák beolvasása|
|/hubs/Providers/Microsoft.Insights/diagnosticSettings/Read|Hello hello erőforrás diagnosztikai beállításának beolvasása|
|/hubs/Providers/Microsoft.Insights/diagnosticSettings/Write|Létrehozza vagy frissíti az hello hello erőforrás diagnosztikai beállításának|
|/hubs/Providers/Microsoft.Insights/logDefinitions/Read|Az erőforrás hello naplók beolvasása|
|/hubs/authorizationPolicies/Read|Azure felhasználói elemzéseket megosztott hozzáférési aláírást házirend olvasása|
|/hubs/authorizationPolicies/Write|Hozzon létre vagy bármely Azure felhasználói Insights megosztott hozzáférési aláírást házirend frissítése|
|/hubs/authorizationPolicies/DELETE|Bármely Azure felhasználói Insights megosztott hozzáférési aláírást házirend törlése|
|/hubs/authorizationPolicies/regeneratePrimaryKey/Action|Azure felhasználói Insights megosztott hozzáférési aláírást házirend elsődleges kulcs újragenerálása|
|/hubs/authorizationPolicies/regenerateSecondaryKey/Action|Azure felhasználói Insights megosztott hozzáférési aláírást házirend másodlagos kulcs újragenerálása|
|/hubs/Profiles/Read|Bármely Azure felhasználói Insights profil olvasása|
|/hubs/Profiles/Write|Minden Azure felhasználói Insights profil írása|
|/hubs/KPI/read|Bármely Azure felhasználói Insights fő teljesítménymutató olvasása|
|/hubs/KPI/Write|Létrehozása vagy frissítése bármely Azure felhasználói Insights fő teljesítménymutató|
|/hubs/KPI/DELETE|Bármely Azure felhasználói Insights fő teljesítménymutató törlése|
|/hubs/Views/Read|Olvassa el a bármely Azure felhasználói Insights alkalmazás megtekintése|
|/hubs/Views/Write|Hozzon létre vagy bármely Azure felhasználói Insights alkalmazás nézet frissítése|
|/hubs/Views/DELETE|Bármely Azure felhasználói Insights alkalmazás nézet törlése|
|/hubs/interactions/Read|Olvassa el az Azure felhasználói Insights beavatkozás|
|/hubs/interactions/Write|Létrehozása vagy frissítése Insights Azure felhasználói beavatkozás|
|/hubs/roleAssignments/Read|Olvassa el a bármely Azure felhasználói Insights Rbac-hozzárendelés|
|/hubs/roleAssignments/Write|Hozzon létre, vagy bármely Azure felhasználói Insights Rbac-hozzárendelés módosítása|
|/hubs/roleAssignments/DELETE|Bármely Azure felhasználói Insights Rbac-hozzárendelés törlése|
|/hubs/Connectors/Read|Olvassa el a bármely Azure felhasználói Insights-összekötő|
|/hubs/Connectors/Write|Létrehozása vagy frissítése bármely Azure felhasználói Insights-összekötő|
|/hubs/Connectors/DELETE|Bármely Azure felhasználói Insights-összekötő törlése|
|/hubs/Connectors/Mappings/Read|Olvassa el a bármely Azure felhasználói Insights összekötő leképezése|
|/hubs/Connectors/Mappings/Write|Hozzon létre vagy frissítési bármely Azure felhasználói Insights összekötő leképezés|
|/hubs/Connectors/Mappings/DELETE|Bármely Azure felhasználói Insights összekötő leképezés törlése|

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

| Művelet | Leírás |
|---|---|
|/ checkNameAvailability/művelet|Bérlő katalógus neve rendelkezésre állását ellenőrzi.|
|/catalogs/Read|Tulajdonságok beolvasása katalógus vagy a katalógusban az előfizetéshez vagy erőforráscsoporthoz.|
|/ katalógusok/írása|Katalógus vagy a frissítések hello címkék és a Tulajdonságok hello katalógus hoz létre.|
|/catalogs/DELETE|Hello katalógus törli.|

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

| Művelet | Leírás |
|---|---|
|/datafactories/Read|Olvassa be az adat-Előállítóban.|
|/ datafactories/írása|Hozzon létre vagy adat-előállító frissítése|
|/datafactories/DELETE|Törli az adat-Előállítóban.|
|/datafactories/datapipelines/Read|Olvassa be a folyamatot.|
|/datafactories/datapipelines/DELETE|Feldolgozási sor törlése.|
|/datafactories/datapipelines/pause/Action|Adatcsatorna szünetel.|
|/datafactories/datapipelines/Resume/Action|Adatcsatorna folytatása.|
|/datafactories/datapipelines/Update/Action|Frissítések folyamat.|
|/datafactories/datapipelines/Write|Létrehozni vagy frissíteni a feldolgozási sor|
|/datafactories/linkedServices/Read|Beolvassa a társított szolgáltatás.|
|/datafactories/linkedServices/DELETE|Törli a társított szolgáltatás.|
|/datafactories/linkedServices/Write|Hozzon létre vagy frissítés társított szolgáltatás|
|/datafactories/Tables/Read|Beolvassa a táblában.|
|/datafactories/Tables/DELETE|Tábla törlése.|
|/datafactories/Tables/Write|Hozható létre vagy frissíthető tábla|

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

| Művelet | Leírás |
|---|---|
|/accounts/Read|Hello DataLakeAnalytics fiók adatainak beolvasása.|
|/ fiókok/írása|Fiók létrehozása vagy frissítése hello DataLakeAnalytics.|
|/accounts/DELETE|Hello DataLakeAnalytics fiók törlése.|
|/accounts/firewallRules/Read|Egy tűzfalszabály adatainak beolvasása.|
|/accounts/firewallRules/Write|Hozzon létre, vagy egy tűzfalszabály módosítása.|
|/accounts/firewallRules/DELETE|Tűzfalszabály törlése.|
|/accounts/storageAccounts/Read|Csatolt Storage-fiók beolvasása hello DataLakeAnalytics fiók.|
|/accounts/storageAccounts/Write|A tárolási fiók toohello DataLakeAnalytics fiókhoz csatolni.|
|/accounts/storageAccounts/DELETE|A Storage-fiókok a hello DataLakeAnalytics fiók választható.|
|/accounts/storageAccounts/containers/Read|A tárfiók hello tárolók beolvasása|
|/accounts/storageAccounts/containers/listSasTokens/Action|Hello tároló SAS-tokenje listája|
|/accounts/dataLakeStoreAccounts/Read|Hello DataLakeAnalytics fiók csatolt DataLakeStore fiók lekérése.|
|/accounts/dataLakeStoreAccounts/Write|Hivatkozásra egy DataLakeStore fiók toohello DataLakeAnalytics fiók.|
|/accounts/dataLakeStoreAccounts/DELETE|Választható hello DataLakeAnalytics fiók egy DataLakeStore-fiókot.|

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

| Művelet | Leírás |
|---|---|
|/accounts/Read|Egy már DataLakeStore fiók adatainak beolvasása.|
|/ fiókok/írása|Hozzon létre egy új DataLakeStore fiókot, vagy egy már DataLakeStore fiók frissítéséhez.|
|/accounts/DELETE|Már DataLakeStore-fiók törlése.|
|/accounts/firewallRules/Read|Egy tűzfalszabály adatainak beolvasása.|
|/accounts/firewallRules/Write|Hozzon létre, vagy egy tűzfalszabály módosítása.|
|/accounts/firewallRules/DELETE|Tűzfalszabály törlése.|
|/accounts/trustedIdProviders/Read|Egy megbízható identitásszolgáltató adatainak beolvasása.|
|/accounts/trustedIdProviders/Write|Hozzon létre, vagy egy megbízható identitásszolgáltató frissítése.|
|/accounts/trustedIdProviders/DELETE|Egy megbízható identitásszolgáltató törlése.|

## <a name="microsoftdevices"></a>Microsoft.Devices

| Művelet | Leírás |
|---|---|
|/ regisztrációs/művelet|Hello előfizetés regisztrálása a hello IOT hubbal erőforrás-szolgáltató, és lehetővé teszi, hogy hello létrehozása az IOT hubbal erőforrások|
|/ checkNameAvailability/művelet|Ellenőrizze a rendelkezésre álló Ha IOT hubbal név|
|/ módjait/Olvasás|Ezzel előfizetés használati adatokat a szolgáltató.|
|/ műveletek/Olvasás|Minden ResourceProvider műveletek beolvasása|
|/ iotHubs/Olvasás|Lekérdezi a hello IOT hubbal (oka) t|
|/ iotHubs/írása|Létrehozni vagy frissíteni az IOT hubbal erőforrás|
|/ iotHubs/törlése|IOT hubbal erőforrás törlése|
|/iotHubs/listkeys/Action|Minden IOT hubbal kulcs beszerzése|
|/iotHubs/exportDevices/Action|Eszközök exportálása|
|/iotHubs/importDevices/Action|Eszközök importálása|
|IotHubs/metricDefinitions/olvasása|Az IOT hubbal szolgáltatás hello hello elérhető mérőszámok beolvasása|
|/iotHubs/iotHubKeys/listkeys/Action|Hello Utónév az IOT hubbal kulcs beszerzése|
|/iotHubs/iotHubStats/Read|Megtekintheti a statisztikákat IOT hubbal|
|/iotHubs/quotaMetrics/Read|Kvóta metrikák beolvasása|
|/iotHubs/eventHubEndpoints/consumerGroups/Write|Az EventHub felhasználói csoport létrehozása|
|/iotHubs/eventHubEndpoints/consumerGroups/Read|Az EventHub fogyasztói csoportot beolvasása|
|/iotHubs/eventHubEndpoints/consumerGroups/DELETE|Az EventHub felhasználói csoport törlése|
|/iotHubs/Routing/routes/$ testall parancsot/művelet|A tesztüzenet szemben az összes meglévő útvonal|
|/iotHubs/Routing/routes/$ testnew/művelet|A tesztüzenet szemben a megadott útvonal tesztelése|
|IotHubs/diagnosticSettings/olvasása|Hello hello erőforrás diagnosztikai beállításának beolvasása|
|/ IotHubs/diagnosticSettings/írása|Létrehozza vagy frissíti az hello hello erőforrás diagnosztikai beállításának|
|/iotHubs/skus/Read|Érvényes IOT hubbal termékváltozatok beolvasása|
|/iotHubs/Jobs/Read|Ezzel a feladat a megadott IOT hubbal beküldött adatokat|
|/iotHubs/routingEndpointsHealth/Read|Az IOT hubbal az összes útválasztási végpontok hello állapotának beolvasása|

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

| Művelet | Leírás |
|---|---|
|/ Előfizetés/regisztrációs/művelet|Hello az előfizetés regisztrálása|
|/Labs/DELETE|Labs törlése.|
|/Labs/Read|Olvassa el a labs.|
|/ labs/írása|Hozzáadása vagy módosítása labs.|
|/Labs/ListVhds/Action|Listázza az elérhető egyéni lemezkép létrehozásához szükséges lemezképeket.|
|/Labs/GenerateUploadUri/Action|URI generálása egyéni lemez képek tooa labor feltöltése.|
|/Labs/CreateEnvironment/Action|Virtuális gépek létrehozása egy tesztkörnyezetben.|
|/Labs/ClaimAnyVm/Action|Jogcím-hello laborban véletlenszerű claimable virtuális gép.|
|/Labs/ExportResourceUsage/Action|Export hello labor Erőforrás kihasználtsága a tárfiók|
|/Labs/Users/DELETE|Törölje a felhasználói profilokat.|
|/Labs/Users/Read|Olvassa a felhasználói profilokat.|
|/Labs/Users/Write|Hozzáadása vagy módosítása a felhasználói profilok.|
|/Labs/Users/secrets/DELETE|Törölje a titkos kulcsok.|
|/Labs/Users/secrets/Read|Olvassa el a titkos kulcsok.|
|/Labs/Users/secrets/Write|Hozzáadása vagy módosítása a titkos kulcsok.|
|/Labs/Users/Environments/DELETE|Törölje a környezetben.|
|/Labs/Users/Environments/Read|Olvassa el a környezetben.|
|/Labs/Users/Environments/Write|Hozzáadása vagy módosítása a környezetben.|
|/Labs/Users/Disks/DELETE|Törölje a lemezt.|
|/Labs/Users/Disks/Read|Lemezek olvasása.|
|/Labs/Users/Disks/Write|Hozzáadása vagy módosítása a lemezeket.|
|/Labs/Users/Disks/Attach/Action|Csatolja, és hello bérleti hello lemez toohello virtuális gép létrehozása.|
|/Labs/Users/Disks/Detach/Action|Válassza le, break hello bérleti hello lemez csatlakoztatott toohello virtuális gépet.|
|/Labs/customImages/DELETE|Törölje az egyéni lemezképet.|
|/Labs/customImages/Read|Olvassa el az egyéni lemezképek.|
|/Labs/customImages/Write|Hozzáadása vagy módosítása az egyéni lemezképek.|
|/Labs/serviceRunners/DELETE|Szolgáltatás indák törlése.|
|/Labs/serviceRunners/Read|Olvasási szolgáltatás indák.|
|/Labs/serviceRunners/Write|Adja hozzá, vagy módosítsa a szolgáltatás indák.|
|/Labs/artifactSources/DELETE|Összetevő források törlése.|
|/Labs/artifactSources/Read|Olvassa el a összetevő források.|
|/Labs/artifactSources/Write|Hozzáadása vagy módosítása összetevő források.|
|/Labs/artifactSources/artifacts/Read|Olvassa el az összetevők.|
|/Labs/artifactSources/artifacts/GenerateArmTemplate/Action|Egy ARM-sablon a megadott összetevő hello állít elő, hello szükséges fájlok tooa tárfiók feltölti és generált hello összetevő ellenőrzi.|
|/Labs/artifactSources/armTemplates/Read|Olvassa el az azure resource manager-sablonok.|
|/Labs/Costs/Read|Olvassa el a költségeket.|
|/Labs/Costs/Write|Adja hozzá, és módosíthatja az.|
|/Labs/virtualNetworks/DELETE|Törölje a virtuális hálózatok.|
|/Labs/virtualNetworks/Read|Olvassa el a virtuális hálózatok.|
|/Labs/virtualNetworks/Write|Adja hozzá, vagy módosítsa a virtuális hálózatok.|
|/Labs/Formulas/DELETE|Képletek törlése.|
|/Labs/Formulas/Read|Olvassa el a formulákat.|
|/Labs/Formulas/Write|Hozzáadása vagy módosítása formulákat.|
|/Labs/schedules/DELETE|Törölni az ütemezéseket.|
|/Labs/schedules/Read|Olvassa el az ütemezések.|
|/Labs/schedules/Write|Hozzáadása vagy módosítása ütemezések.|
|/Labs/schedules/Execute/Action|Ütemezés hajtható végre.|
|/Labs/schedules/ListApplicable/Action|Felsorolja az összes megfelelő ütemezést|
|/Labs/galleryImages/Read|Olvassa el a gyűjtemény lemezképei.|
|/Labs/policySets/EvaluatePolicies/Action|Labor házirend kiértékelése.|
|/Labs/policySets/Policies/DELETE|Törölje a házirendeket.|
|/Labs/policySets/Policies/Read|Olvassa el a házirendeket.|
|/Labs/policySets/Policies/Write|Adja hozzá vagy módosíthat házirendeket.|
|/Labs/virtualMachines/DELETE|Törölje a virtuális gépeket.|
|/Labs/virtualMachines/Read|Olvassa el a virtuális gépek.|
|/Labs/virtualMachines/Write|Virtuális gépek hozzáadása vagy módosítása.|
|/Labs/virtualMachines/Start/Action|Indítsa el a virtuális gépet.|
|/Labs/virtualMachines/STOP/Action|A virtuális gép leállítása|
|/Labs/virtualMachines/ApplyArtifacts/Action|Az összetevők toovirtual gép alkalmazni.|
|/Labs/virtualMachines/AddDataDisk/Action|Egy új vagy meglévő lemez toovirtual géphez csatolása.|
|/Labs/virtualMachines/DetachDataDisk/Action|Hello megadott lemezt leválasztani a virtuális gép hello.|
|/Labs/virtualMachines/Claim/Action|Saját tulajdonba vétele meglévő virtuális gépből|
|/Labs/virtualMachines/ListApplicableSchedules/Action|Felsorolja az összes megfelelő ütemezést|
|/Labs/virtualMachines/schedules/DELETE|Törölni az ütemezéseket.|
|/Labs/virtualMachines/schedules/Read|Olvassa el az ütemezések.|
|/Labs/virtualMachines/schedules/Write|Hozzáadása vagy módosítása ütemezések.|
|/Labs/virtualMachines/schedules/Execute/Action|Ütemezés hajtható végre.|
|/Labs/notificationChannels/DELETE|Notificationchannels törlése.|
|/Labs/notificationChannels/Read|Olvassa el a notificationchannels.|
|/Labs/notificationChannels/Write|Hozzáadása vagy módosítása notificationchannels.|
|/Labs/notificationChannels/Notify/Action|Értesítési tooprovided csatorna küldése.|
|/schedules/DELETE|Törölni az ütemezéseket.|
|/schedules/Read|Olvassa el az ütemezések.|
|/ ütemezések/írása|Hozzáadása vagy módosítása ütemezések.|
|/schedules/Execute/Action|Ütemezés hajtható végre.|
|/schedules/Retarget/Action|Frissíti a ütemezés célerőforrás azonosítója.|
|/Locations/Operations/Read|Az olvasási műveletek.|

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

| Művelet | Leírás |
|---|---|
|/databaseAccountNames/Read|A név foglaltságának ellenőrzése.|
|/databaseAccounts/Read|Az adatbázisfiók beolvasása.|
|/ databaseAccounts/írása|Egy adatbázis-fiók frissítéséhez.|
|/databaseAccounts/listKeys/Action|Egy adatbázis-fiók listázása|
|/databaseAccounts/regenerateKey/Action|Az adatbázisfiók kulcsokkal elforgatása|
|/databaseAccounts/listConnectionStrings/Action|Az adatbázisfiók hello kapcsolati karakterláncok beolvasása|
|/databaseAccounts/changeResourceGroup/Action|Módosítsa az erőforráscsoport egy olyan adatbázis-fiók|
|/databaseAccounts/failoverPriorityChange/Action|Az adatbázisfiók régiók feladatátvételi prioritás módosítása. Ez az használt tooperform kézi feladatátvételi művelet|
|/databaseAccounts/DELETE|Hello adatbázis fiókok törlése.|
|/databaseAccounts/metricDefinitions/Read|Hello adatbázisfiók metrikák meghatározások beolvasása.|
|/databaseAccounts/Metrics/Read|Hello adatbázis fiók metrikák beolvasása.|
|/databaseAccounts/usages/Read|Beolvassa a hello adatbázis fiók módjait.|
|/databaseAccounts/Databases/Collections/metricDefinitions/Read|Hello gyűjtemény metrikai meghatározásainak beolvasása.|
|/databaseAccounts/Databases/Collections/Metrics/Read|Hello gyűjtemény metrikák beolvasása.|
|/databaseAccounts/Databases/Collections/usages/Read|Beolvassa a hello gyűjtemény módjait.|
|/databaseAccounts/Databases/metricDefinitions/Read|Hello adatbázis metrikai meghatározásainak beolvasása|
|/databaseAccounts/Databases/Metrics/Read|Hello adatbázis metrikák beolvasása.|
|/databaseAccounts/Databases/usages/Read|Beolvassa a hello adatbázis módjait.|
|/databaseAccounts/readonlykeys/Read|Hello adatbázisfiók írásvédett kulcsok beolvasása.|

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

| Művelet | Leírás |
|---|---|
|/ generateSsoRequest/művelet|Létre a tartomány vezérlőközpont bejelentkezni.|
|/ validateDomainRegistrationInformation/művelet|Ellenőrizze a tartomány beszerzési objektum lefolytatása nélkül|
|/ checkDomainAvailability/művelet|Ellenőrizze, hogy a tartomány megvásárolható|
|/ listDomainRecommendations/művelet|Hello lista tartomány meg ajánlásainkat kulcsszavak beolvasása|
|/ regisztrációs/művelet|Hello Microsoft Domains erőforrás-szolgáltató hello előfizetés regisztrálása|
|/ tartományok/Olvasás|Hello szereplő tartományok listájának beolvasása|
|/ tartományok/írása|Új tartomány hozzáadása vagy egy meglévő frissítése|
|/ tartományok/törlése|Törölje a meglévő tartományban.|
|/Domains/operationresults/Read|A tartomány művelet|

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

| Művelet | Leírás |
|---|---|
|/lcsprojects/Read|A Microsoft Dynamics életciklus szolgáltatások projektek tooa felhasználói tartozó megjelenítéséhez|
|/ lcsprojects/írása|Létrehozása és frissítése a Microsoft Dynamics életciklus szolgáltatások projektek toohello felhasználói tartoznak. Csak hello nevének és leírásának tulajdonságait is lehet frissíteni. hello előfizetésekhez és helyekhez társított hello projekt létrehozása után nem frissíthető|
|/lcsprojects/DELETE|A Microsoft Dynamics életciklus szolgáltatások projektek tartozó toohello felhasználó törlése|
|/lcsprojects/clouddeployments/Read|Megjeleníti a Microsoft Dynamics AX 2012 R3 értékelési központi telepítések tooa felhasználói tartozó Microsoft Dynamics életciklus szolgáltatások projektben|
|/lcsprojects/clouddeployments/Write|Microsoft Dynamics AX 2012 R3 értékelési központi telepítés létrehozásához tartozó tooa felhasználói Microsoft Dynamics életciklus szolgáltatások projektben. Központi telepítések is kezelhető az Azure felügyeleti portálon|
|/lcsprojects/Connectors/Read|Olvassa el, amelyek Microsoft Dynamics életciklus Services-projekt tooa összekötők|
|/lcsprojects/Connectors/Write|Létrehozása és frissítése, amelyek Microsoft Dynamics életciklus Services-projekt tooa összekötők|

## <a name="microsofteventhub"></a>Microsoft.EventHub

| Művelet | Leírás |
|---|---|
|/ checkNameAvailability/művelet|A névtér az adott előfizetésben való elérhetőségének ellenőrzése.|
|/ regisztrációs/művelet|Hello előfizetés hello EventHub erőforrás-szolgáltató regisztrálása, és lehetővé teszi az EventHub erőforrások hello létrehozását|
|/ névterek/írása|Namespace erőforrás létrehozása és frissítése a tulajdonságait. Címkék és a hello Namespace állapota nem frissíthető hello tulajdonságait.|
|/Namespaces/Read|Hello listáját Namespace erőforrás leírása|
|/ névterek/törlése|Namespace erőforrás törlése|
|/Namespaces/metricDefinitions/Read|Namespace metrikák erőforrás leírása listájának beolvasása|
|/Namespaces/authorizationRules/Read|Itt olvashatók névterek az engedélyezési szabályok leírás hello listája.|
|/Namespaces/authorizationRules/Write|A Namespace szintű engedélyezési szabályok létrehozása és frissítése a tulajdonságait. hello engedélyezési szabályok hozzáférési jogosultságokat, hello elsődleges és másodlagos kulcsok lehet frissíteni.|
|/Namespaces/authorizationRules/DELETE|Namespace engedélyezési szabály törlése. hello alapértelmezett Namespace engedélyezési szabály nem törölhető. |
|/Namespaces/authorizationRules/listkeys/Action|Hello kapcsolati karakterlánc toohello Namespace beolvasása|
|/Namespaces/authorizationRules/regenerateKeys/Action|Hello elsődleges vagy másodlagos kulcs toohello erőforrás újragenerálása|
|/Namespaces/eventhubs/Write|Hozzon létre vagy frissítés EventHub tulajdonságai.|
|/Namespaces/eventhubs/Read|Az EventHub erőforrás leírások listáját|
|/Namespaces/eventhubs/DELETE|A művelet toodelete EventHub erőforrás|
|/Namespaces/eventHubs/consumergroups/Write|Hozzon létre vagy frissítés ConsumerGroup tulajdonságai.|
|/Namespaces/eventHubs/consumergroups/Read|Listájának ConsumerGroup erőforrás leírása|
|/Namespaces/eventHubs/consumergroups/DELETE|A művelet toodelete ConsumerGroup erőforrás|
|/Namespaces/eventhubs/authorizationRules/Read| Hello listájának EventHub az engedélyezési szabályok|
|/Namespaces/eventhubs/authorizationRules/Write|Az EventHub-engedélyezési szabályok létrehozása és frissítése a tulajdonságait. hello engedélyezési szabályok hozzáférési jogosultságokat, hello elsődleges és másodlagos kulcsok lehet frissíteni.|
|/Namespaces/eventhubs/authorizationRules/DELETE|A művelet toodelete EventHub-engedélyezési szabályok|
|/Namespaces/eventhubs/authorizationRules/listkeys/Action|Hello kapcsolati karakterlánc tooEventHub beolvasása|
|/Namespaces/eventhubs/authorizationRules/regenerateKeys/Action|Hello elsődleges vagy másodlagos kulcs toohello erőforrás újragenerálása|
|/Namespaces/diagnosticSettings/Read|Namespace diagnosztikai beállítások erőforrás leírása listájának beolvasása|
|/Namespaces/diagnosticSettings/Write|Namespace diagnosztikai beállítások erőforrás leírása listájának beolvasása|
|/Namespaces/logDefinitions/Read|Listájának Namespace naplók erőforrás leírása|

## <a name="microsoftfeatures"></a>Microsoft.Features

| Művelet | Leírás |
|---|---|
|/Providers/features/Read|Lekérdezi az előfizetés hello szolgáltatást az adott erőforrás-szolgáltatón.|
|/Providers/features/register/Action|Regisztrálja az előfizetéshez tartozó hello szolgáltatást az adott erőforrás-szolgáltatón.|
|/features/Read|Lekérdezi az előfizetéshez tartozó hello szolgáltatásokat.|

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

| Művelet | Leírás |
|---|---|
|/ fürtök/írása|Létrehozni vagy frissíteni a HDInsight-fürt|
|/Clusters/Read|Részletes információkat szolgáltatva HDInsight-fürt|
|/Clusters/DELETE|A HDInsight-fürt törlése|
|/Clusters/changerdpsetting/Action|HDInsight-fürt RDP beállításának módosítása|
|/Clusters/configurations/Action|HDInsight-fürt konfigurációjának frissítése|
|/Clusters/configurations/Read|HDInsight-fürt konfigurációjának beolvasása|
|/Clusters/Roles/Resize/Action|A HDInsight-fürt méretezése|
|/Locations/Capabilities/Read|Az előfizetési lehetőségek elérése|
|/Locations/checkNameAvailability/Read|Név foglaltságának ellenőrzése|

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

| Művelet | Leírás |
|---|---|
|/ regisztrációs/művelet|Hello előfizetés hello importálási/exportálási erőforrás-szolgáltató regisztrálása és az importálási/exportálási feladatok hello létrehozása.|
|/ feladatok/írása|Létrehoz egy feladatot a megadott paraméterek hello vagy hello tulajdonságok vagy a címkék hello megadott feladathoz tartozó frissítés.|
|/Jobs/Read|Hello tulajdonságok lekérdezi a megadott feladat hello, vagy hello feladatok listáját adja vissza.|
|/Jobs/listBitLockerKeys/Action|Lekérdezi a BitLocker-kulcsok hello hello megadott feladathoz tartozó.|
|/Jobs/DELETE|Egy meglévő feladat törlése.|
|/Locations/Read|Hello tulajdonságok hello megadott helyre vagy értéket ad vissza hello helyek listájának beolvasása.|

## <a name="microsoftinsights"></a>Microsoft.Insights

| Művelet | Leírás |
|---|---|
|/ Regisztrációs/művelet|A microsoft hello insights szolgáltató regisztrálása|
|/ AlertRules/írása|Írás tooan riasztási szabály konfigurálása|
|Vagy AlertRules/törlése|Riasztási szabály konfigurációjának törlése|
|AlertRules/olvasása|Riasztási szabály konfigurációjának beolvasása|
|/ AlertRules/aktiválva/művelet|Riasztási szabály aktiválva|
|/ AlertRules/feloldva/művelet|Riasztási szabály feloldása|
|/ AlertRules/Halmozódni/művelet|Riasztási szabály szabályozva van|
|AlertRules/incidensek/olvasása|Riasztási szabály incidenskonfigurációjának beolvasása|
|MetricDefinitions/olvasása|Olvassa el a metrikai meghatározásainak|
|/eventtypes/Values/Read|A felügyeleti eseménytípus értékeinek olvasása|
|/eventtypes/digestevents/Read|A felügyeleti eseménytípus kivonatának olvasása|
|Metrikák/olvasása|Olvassa el a metrikák|
|/ LogProfiles/írása|Tooa napló profil konfigurációs írása|
|Vagy LogProfiles/törlése|Naplóbeállítások profilok törlése|
|LogProfiles/olvasása|Olvasási napló profilok|
|/ AutoscaleSettings/írása|Tooan automatikus skálázási beállítás konfigurációs írása|
|Vagy AutoscaleSettings/törlése|Automatikus skálázási beállítás konfigurációjának törlése|
|AutoscaleSettings/olvasása|Automatikus skálázási beállítás konfigurációjának beolvasása|
|/ AutoscaleSettings/Scaleup/művelet|Automatikus vertikális felskálázási művelet|
|/ AutoscaleSettings/Scaledown/művelet|Automatikus skálázás skálázási művelet le|
|/AutoscaleSettings/Providers/Microsoft.Insights/MetricDefinitions/Read|Olvassa el a metrikai meghatározásainak|
|/ ActivityLogAlerts/aktiválva/művelet|Tevékenység napló riasztási kiváltott hello|
|/ DiagnosticSettings/írása|Írás toodiagnostic beállítások|
|Vagy DiagnosticSettings/törlése|Diagnosztikai beállítások konfiguráció törlése|
|DiagnosticSettings/olvasása|Diagnosztikai beállítások konfigurációjának olvasása|
|LogDefinitions/olvasása|Olvasási naplófájl-definíciói|
|/ ExtendedDiagnosticSettings/írása|Írás tooextended diagnosztikai beállítások|
|Vagy ExtendedDiagnosticSettings/törlése|Bővített diagnosztikai beállítások konfiguráció törlése|
|ExtendedDiagnosticSettings/olvasása|Bővített diagnosztikai beállítások beolvasásakor|

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

| Művelet | Leírás |
|---|---|
|/ regisztrációs/művelet|Egy előfizetés regisztrálása|
|/checkNameAvailability/Read|Annak ellenőrzése, hogy érvényes-e a kulcstartónév, és nincs-e használatban|
|/vaults/Read|Kulcstároló hello tulajdonságainak megtekintése|
|/ tárolók/írása|Hozzon létre egy új kulcs tárolóhoz vagy a frissítés hello egy meglévő kulcstároló tulajdonságainak|
|/vaults/DELETE|Kulcstároló törlése|
|/vaults/Deploy/Action|Lehetővé teszi, hogy a kulcstároló toosecrets érhető el, amikor üzembe helyezése az Azure-erőforrások|
|/vaults/secrets/Read|A titkos kulcsot, de nem értéke hello tulajdonságainak megtekintése|
|/vaults/secrets/Write|Hozzon létre egy új titkos kulcs vagy frissítés hello értéket egy meglévő titok|
|/vaults/accessPolicies/Write|A meglévő hozzáférési házirendek frissítéséhez az egyesítés vagy cseréje, vagy egy új hozzáférési házirend tooa tárolót.|
|/deletedVaults/Read|Letölthető törölt kulcstárolójának hello tulajdonságainak megtekintése|
|/Locations/operationResults/Read|Ellenőrizze a művelet hosszú futtatásukkor hello eredménye|
|/Locations/deletedVaults/Read|Hello enyhe törölt kulcstároló tulajdonságainak megtekintése|
|/Locations/deletedVaults/PURGE/Action|Letölthető törölt kulcstároló törlése|

## <a name="microsoftlogic"></a>Microsoft.Logic

| Művelet | Leírás |
|---|---|
|/workflows/Read|Hello munkafolyamat beolvasása.|
|/ munkafolyamatok/írása|Létrehozza vagy frissíti a hello munkafolyamat.|
|/workflows/DELETE|Hello munkafolyamat törli.|
|/workflows/Run/Action|Elindítja a hello munkafolyamat futását.|
|/workflows/disable/Action|Letiltja a hello munkafolyamat.|
|/workflows/enable/Action|Lehetővé teszi, hogy hello munkafolyamat.|
|/workflows/Validate/Action|Hello munkafolyamat ellenőrzi.|
|/workflows/MOVE/Action|A meglévő előfizetés-azonosító, erőforráscsoport és/vagy név tooa eltérő előfizetés-azonosító, erőforráscsoport és/vagy nevét az áll át, a munkafolyamat.|
|/workflows/listSwagger/Action|Hello munkafolyamat lekérdezi a swagger-definíciók.|
|/workflows/regenerateAccessKey/Action|Hello hozzáférési kulcs titkos kulcsok újragenerálása.|
|/workflows/listCallbackUrl/Action|Munkafolyamat hello visszahívási URL-címet lekéri.|
|/workflows/versions/Read|Beolvassa a hello munkafolyamat-verzió.|
|/workflows/versions/triggers/listCallbackUrl/Action|Hello visszahívási URL-cím lekérése eseményindító.|
|/ munkafolyamatok/fut/olvasása|Beolvassa a hello munkafolyamat futtatásához.|
|/workflows/runs/Cancel/Action|Egy munkafolyamat futtatásakor hello visszavonása.|
|/workflows/runs/actions/Read|Hello munkafolyamat művelet beolvassa.|
|/workflows/runs/Operations/Read|Hello munkafolyamat műveleti állapotának beolvasása.|
|/workflows/triggers/Read|Hello eseményindító beolvasása.|
|/workflows/triggers/Run/Action|Hello eseményindító végrehajtása.|
|/workflows/triggers/listCallbackUrl/Action|Hello visszahívási URL-cím lekérése eseményindító.|
|/workflows/triggers/histories/Read|Hello eseményindító alábbi előzményeinek beolvasása.|
|/workflows/triggers/histories/resubmit/Action|Megismétli hello munkafolyamat eseményindító.|
|/workflows/accessKeys/Read|Beolvassa a hello a hozzáférési kulcsot.|
|/workflows/accessKeys/Write|Létrehozza vagy frissíti a hello a hozzáférési kulcsot.|
|/workflows/accessKeys/DELETE|Hello elérési kulcs törlése.|
|/workflows/accessKeys/List/Action|Hello hozzáférési kulcs titkos kulcsok sorolja fel.|
|/workflows/accessKeys/regenerate/Action|Hello hozzáférési kulcs titkos kulcsok újragenerálása.|
|/Locations/workflows/Validate/Action|Hello munkafolyamat ellenőrzi.|

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

| Művelet | Leírás |
|---|---|
|/ regisztrációs/művelet|Hello machine learning web service erőforrás-szolgáltató hello előfizetés regisztrálja, és lehetővé teszi a webszolgáltatások hello létrehozását.|
|/ webServices/művelet|A támogatott régiók regionális webes szolgáltatás tulajdonságok létrehozása|
|/commitmentPlans/Read|Olvassa el a gépi tanulási kötelezettségvállalás terv|
|/ commitmentPlans/írása|Hozzon létre vagy bármely Machine Learning előfizetési csomag frissítése|
|/commitmentPlans/DELETE|Gépi tanulás kötelezettségvállalás terv törlése|
|/commitmentPlans/JOIN/Action|Csatlakozás a gépi tanulási kötelezettségvállalás terv|
|/commitmentPlans/commitmentAssociations/Read|Olvassa el a gépi tanulási kötelezettségvállalás terv társítása|
|/commitmentPlans/commitmentAssociations/MOVE/Action|Helyezze át a gépi tanulási kötelezettségvállalás terv társítása|
|A munkaterületek között/olvasása|Olvassa el a gépi tanulási munkaterület|
|A munkaterületek között/írása|Létrehozni vagy frissíteni a gépi tanulási munkaterület|
|Vagy munkaterületek/törlése|Gépi tanulás munkaterület törlése|
|/ Munkaterületek/listworkspacekeys/művelet|A Machine Learning-munkaterület listában kulcsok|
|/ Munkaterületek/resyncstoragekeys/művelet|A Machine Learning-munkaterület beállítása tárfiók kulcsait újraszinkronizálásra|
|/WebServices/Read|Olvassa el a Machine Learning webszolgáltatás|
|/ webServices/írása|Létrehozni vagy frissíteni a Machine Learning webszolgáltatás|
|/WebServices/DELETE|A Machine Learning webszolgáltatás törlése|

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

| Művelet | Leírás |
|---|---|
|/agreements/offers/plans/Read|Térjen vissza az adott piactér elem szerződés|
|/agreements/offers/plans/Sign/Action|Egy adott piactér elem létrejött|
|/agreements/offers/plans/Cancel/Action|Szakítsa meg a szerződés egy adott piactér elem|

## <a name="microsoftmedia"></a>Microsoft.Media

| Művelet | Leírás |
|---|---|
|/mediaservices/Read||
|/ mediaservices/írása||
|/mediaservices/DELETE||
|/mediaservices/regenerateKey/Action||
|/mediaservices/listKeys/Action||
|/mediaservices/syncStorageKeys/Action||

## <a name="microsoftnetwork"></a>Microsoft.Network

| Művelet | Leírás |
|---|---|
|/ regisztrációs/művelet|Hello az előfizetés regisztrálása|
|/ unregister/művelet|Hello előfizetés regisztrációjának törlése|
|/ checkTrafficManagerNameAvailability/művelet|A Traffic Manager relatív DNS-név hello rendelkezésre állását ellenőrzi.|
|/dnszones/Read|Első hello DNS-zóna JSON formátumban. hello zóna tulajdonságai tartalmaznak címkéket, etag, numberOfRecordSets és maxnumberofrecordsets tulajdonságokat. Vegye figyelembe, hogy ez a parancs nem kéri le a hello rekordhalmazok hello zónán belül található.|
|/ dnszones/írása|Hozzon létre, vagy frissítse a DNS-zónák erőforráscsoporton belül.  A DNS-zóna erőforrás használt tooupdate hello címke. Vegye figyelembe, hogy ez a parancs nem lehet használt toocreate vagy frissítés rekordhalmazok hello zónán belül.|
|/dnszones/DELETE|Törölje a hello DNS-zóna JSON formátumban. hello zóna tulajdonságai tartalmaznak címkéket, etag, numberOfRecordSets és maxnumberofrecordsets tulajdonságokat.|
|/dnszones/MX/Read|"MX", az hello típusú rekordkészlet beolvasása JSON formátumban. hello rekordkészlet tartalmazza a rekordok, valamint a hello TTL listája, a címkéket és az etag.|
|/dnszones/MX/Write|Hozzon létre vagy frissíthető a DNS-zónában "MX" típusú. hello rögzíti a megadott hello rekordhalmaz hello művelettel lecseréli.|
|/dnszones/MX/DELETE|Távolítsa el az adott nevű hello rekordhalmaz, és írja be az "MX" a DNS-zónából.|
|/dnszones/NS/Read|DNS NS típusú rekordkészlet beolvasása|
|/dnszones/NS/Write|Létrehozza vagy frissíti a DNS NS típusú rekordhalmaz|
|/dnszones/NS/DELETE|Törli az NS típusú hello DNS rekordkészlete|
|/dnszones/AAAA/Read|"AAAA", az hello típusú rekordkészlet beolvasása JSON formátumban. hello rekordkészlet tartalmazza a rekordok, valamint a hello TTL listája, a címkéket és az etag.|
|/dnszones/AAAA/Write|Hozzon létre vagy frissíthető a DNS-zónában "AAAA" típusú. hello rögzíti a megadott hello rekordhalmaz hello művelettel lecseréli.|
|/dnszones/AAAA/DELETE|Távolítsa el az adott nevű hello rekordhalmaz, és írja be a "AAAA" a DNS-zónából.|
|/dnszones/CNAME/Read|"CNAME", az hello típusú rekordkészlet beolvasása JSON formátumban. hello rekordkészlet tartalmazza a hello TTL-t, a címkéket és az etag.|
|/dnszones/CNAME/Write|Hozzon létre vagy frissíthető a DNS-zónában "CNAME" típusú. hello rögzíti a megadott hello rekordhalmaz hello művelettel lecseréli.|
|/dnszones/CNAME/DELETE|Távolítsa el az adott nevű hello rekordhalmaz, és írja be a "CNAME" a DNS-zónából.|
|/dnszones/SOA/Read|DNS SOA típusú rekordkészlet beolvasása|
|/dnszones/SOA/Write|Létrehozza vagy frissíti a DNS-rekordhalmaz SOA típusú|
|/dnszones/SRV/Read|"SRV", az hello típusú rekordkészlet beolvasása JSON formátumban. hello rekordkészlet tartalmazza a rekordok, valamint a hello TTL listája, a címkéket és az etag.|
|/dnszones/SRV/Write|SRV típusú rekordot csoport létrehozása vagy frissítése|
|/dnszones/SRV/DELETE|Távolítsa el az adott nevű hello rekordhalmaz, és írja be a "SRV" a DNS-zónából.|
|/dnszones/PTR/Read|"PTR", az hello típusú rekordkészlet beolvasása JSON formátumban. hello rekordkészlet tartalmazza a rekordok, valamint a hello TTL listája, a címkéket és az etag.|
|/dnszones/PTR/Write|Hozzon létre vagy frissíthető a DNS-zónában "PTR" típusú. hello rögzíti a megadott hello rekordhalmaz hello művelettel lecseréli.|
|/dnszones/PTR/DELETE|Távolítsa el az adott nevű hello rekordhalmaz, és írja be a "PTR" a DNS-zónából.|
|/dnszones/A/Read|Hello "A" típusú rekordkészlet beolvasása JSON formátumban. hello rekordkészlet tartalmazza a rekordok, valamint a hello TTL listája, a címkéket és az etag.|
|/dnszones/A/Write|Hozzon létre vagy frissíthető a DNS-zónában "A" típusú. hello rögzíti a megadott hello rekordhalmaz hello művelettel lecseréli.|
|/dnszones/A/DELETE|Távolítsa el az adott nevű hello rekordhalmaz, és írja be a "A" a DNS-zónából.|
|/dnszones/txt/Read|"TXT", az hello típusú rekordkészlet beolvasása JSON formátumban. hello rekordkészlet tartalmazza a rekordok, valamint a hello TTL listája, a címkéket és az etag.|
|/dnszones/txt/Write|Hozzon létre vagy frissíthető a DNS-zónában "TXT" típusú. hello rögzíti a megadott hello rekordhalmaz hello művelettel lecseréli.|
|/dnszones/txt/DELETE|Távolítsa el az adott nevű hello rekordhalmaz, és írja be a "TXT" a DNS-zónából.|
|/dnszones/recordsets/Read|Lekérdezi a DNS-rekordhalmazok típusok között|
|/networkInterfaces/Read|Hálózati illesztő definíciójának beolvasása. |
|/ hálózati illesztők/írása|Egy adott hálózati csatoló létrehozza vagy frissíti a meglévő hálózati illesztő. |
|/networkInterfaces/JOIN/Action|A virtuális gép tooa hálózati illesztő csatlakozik|
|/networkInterfaces/DELETE|Hálózati kapcsolat törlése|
|/networkInterfaces/effectiveRouteTable/Action|A virtuális gép hello hálózati illesztőjén konfigurált útválasztási táblázatot beolvasása|
|/networkInterfaces/effectiveNetworkSecurityGroups/Action|Hálózati biztonsági csoportok konfigurált a hálózati illesztő: hello virtuális gép beolvasása|
|/networkInterfaces/loadBalancers/Read|Lekérdezi az összes hello terheléselosztók hálózati illesztő hello része|
|/networkInterfaces/ipconfigurations/Read|A hálózati illesztő ip-konfiguráció definíciójának beolvasása |
|/publicIPAddresses/Read|A nyilvános IP-cím definíciójának beolvasása|
|/ publicIPAddresses/írása|A nyilvános IP-cím létrehozza vagy frissíti a meglévő nyilvános IP-címnek. |
|/publicIPAddresses/DELETE|Törli a nyilvános IP-cím.|
|/publicIPAddresses/JOIN/Action|A nyilvános IP-cím illesztése|
|/routeFilters/Read|Útvonal szűrő definíciójának beolvasása|
|/routeFilters/JOIN/Action|Útvonal szűrő illesztése|
|/routeFilters/DELETE|Egy útvonal szűrődefinícióban törlése|
|/ routeFilters/írása|Útvonal szűrő létrehoz vagy frissít egy meglévő rotue szűrő|
|/routeFilters/Rules/Read|Útvonal szűrési szabály definíciójának beolvasása|
|/routeFilters/Rules/Write|Egy útvonal-szűrő szabályt hoz létre, vagy meglévő útvonal szűrési szabály frissítése|
|/routeFilters/Rules/DELETE|Törli a útvonal szűrő szabályát leíró definíció beolvasása|
|/networkWatchers/Read|Hello hálózati figyelő definíciójának beolvasása|
|/ networkWatchers/írása|A hálózati figyelőt létrehoz vagy frissít egy meglévő hálózati figyelőt|
|/networkWatchers/DELETE|Törli a hálózati figyelőt|
|/networkWatchers/configureFlowLog/Action|Konfigurálja a cél erőforráson folyamat naplózását.|
|/networkWatchers/ipFlowVerify/Action|E hello csomag engedélyezett vagy tiltott tooor egy adott célra adja vissza.|
|/networkWatchers/nextHop/Action|A megadott cél és a cél IP-cím térjen vissza a hello következő ugrás típusa, és ezután az IP-cím legyen.|
|/networkWatchers/queryFlowLogStatus/Action|Erőforrás-naplózás folyamata hello állapotának beolvasása.|
|/networkWatchers/queryTroubleshootResult/Action|Lekérdezi a korábban már futtattak hello vagy a jelenleg futó művelet hibaelhárítási kapott eredmény hibaelhárítási hello.|
|/networkWatchers/securityGroupView/Action|Nézet hello konfigurálva és hatékony hálózati biztonsági csoportszabályok alkalmazása a virtuális gép.|
|/networkWatchers/topology/Action|Lekérdezi a hálózati szintű áttekintés a erőforrásokat, és azok erőforráscsoportban.|
|/networkWatchers/troubleshoot/Action|Egy hálózati erőforráshoz az Azure-ban végzett hibaelhárítás indítása.|
|/networkWatchers/packetCaptures/queryStatus/Action|Lekérdezi a tulajdonságok és a csomag rögzítési erőforrás állapotával kapcsolatos adatokat.|
|/networkWatchers/packetCaptures/STOP/Action|Állítsa le a futó csomag rögzítési munkamenet hello.|
|/networkWatchers/packetCaptures/Read|Hello csomagok rögzítési definíciójának beolvasása|
|/networkWatchers/packetCaptures/Write|A csomagrögzítéssel létrehozása|
|/networkWatchers/packetCaptures/DELETE|A csomagrögzítéssel törlése|
|/loadBalancers/Read|Terheléselosztó definíciójának beolvasása|
|/ loadBalancers/írása|Terheléselosztó létrehozása vagy meglévő terheléselosztó frissítése|
|/loadBalancers/DELETE|Törli a terheléselosztó|
|/loadBalancers/networkInterfaces/Read|Hivatkozások tooall hello hálózati adapterek a terheléselosztók beolvasása|
|/loadBalancers/loadBalancingRules/Read|Terheléselosztó terheléselosztási terheléselosztási szabály definíciójának beolvasása|
|/loadBalancers/backendAddressPools/Read|A load balancer háttércímkészlet definíciójának beolvasása|
|/loadBalancers/backendAddressPools/JOIN/Action|A load balancer háttércímkészletének illesztése|
|/loadBalancers/inboundNatPools/Read|Terheléselosztó bejövő forgalmat kezelő nat-készlet definíciójának beolvasása|
|/loadBalancers/inboundNatPools/JOIN/Action|Művelettel illeszthető egy terheléselosztó bejövő forgalmat kezelő nat-készlete|
|/loadBalancers/inboundNatRules/Read|Terheléselosztó bejövő forgalmat kezelő nat-szabályát leíró definíció beolvasása|
|/loadBalancers/inboundNatRules/Write|Terheléselosztó bejövő forgalmat kezelő nat-szabályának létrehozása vagy frissítése egy meglévő terheléselosztó bejövő nat-szabálya|
|/loadBalancers/inboundNatRules/DELETE|Terheléselosztó bejövő forgalmat kezelő nat-szabályának törlése|
|/loadBalancers/inboundNatRules/JOIN/Action|Terheléselosztó bejövő forgalmat kezelő nat-szabályának illesztése|
|/loadBalancers/outboundNatRules/Read|Terheléselosztó kimenő forgalmat kezelő nat szabály definíciójának beolvasása|
|/loadBalancers/probes/Read|Terheléselosztói mintavétel beolvasása|
|/loadBalancers/virtualMachines/Read|Az egy terheléselosztóhoz tooall hello virtuális gépeket hivatkozások beolvasása|
|/loadBalancers/frontendIPConfigurations/Read|A terheléselosztó előtérbeli IP konfiguráció definíciójának beolvasása|
|/trafficManagerGeographicHierarchies/Read|Lekérdezi a Traffic Manager Geographic hierarchia hello tartalmazó régiókban, amelyek hello földrajzi forgalom-útválasztási módszert együtt|
|/bgpServiceCommunities/Read|Bgp-szolgáltatás Közösségek beolvasása|
|/applicationGatewayAvailableWafRuleSets/Read|Rendelkezésre álló Waf beállítására Alkalmazásátjáró beolvasása|
|/virtualNetworks/Read|Hello virtuális hálózat definíciójának beolvasása|
|/ virtualNetworks/írása|Létrehoz egy virtuális hálózatot, vagy meglévő virtuális hálózat frissítése|
|/virtualNetworks/DELETE|Virtuális hálózat törlése|
|/virtualNetworks/Peer/Action|A virtuális hálózat egy másik virtuális hálózathoz állomásokhoz|
|/virtualNetworks/virtualNetworkPeerings/Read|Virtuális hálózati társviszony-létesítési definíciójának beolvasása|
|/virtualNetworks/virtualNetworkPeerings/Write|Virtuális hálózati társviszony-létesítés létrehozása vagy frissítése. egy meglévő virtuális hálózati társviszony|
|/virtualNetworks/virtualNetworkPeerings/DELETE|Törli a virtuális hálózati társviszony-létesítés|
|/virtualNetworks/Subnets/Read|Virtuális hálózati alhálózat definíciójának beolvasása|
|/virtualNetworks/Subnets/Write|Virtuális hálózati alhálózat létrehozása vagy egy meglévő virtuális hálózati alhálózat frissítése|
|/virtualNetworks/Subnets/DELETE|Virtuális hálózati alhálózat törlése|
|/virtualNetworks/Subnets/JOIN/Action|Egy virtuális hálózathoz csatlakozik|
|/virtualNetworks/Subnets/joinViaServiceTunnel/Action|Erőforrás csatlakozik, például a tárfiók vagy SQL adatbázis-szolgáltatás Tunneling engedélyezve alhálózati tooa.|
|/virtualNetworks/Subnets/virtualMachines/Read|Hivatkozások tooall hello virtuális gépek virtuális hálózati alhálózat beolvasása|
|/virtualNetworks/checkIpAddressAvailability/Read|Ellenőrizze, hogy az IP-cím hello megadott virtuális hálózati érhető el|
|/virtualNetworks/virtualMachines/Read|A virtuális hálózat tooall hello virtuális gépek hivatkozások beolvasása|
|/expressRouteServiceProviders/Read|Lekérdezi az Express Route-szolgáltatók|
|/dnsoperationresults/Read|A DNS-művelet eredményének beolvasása|
|/localnetworkgateways/Read|Lekérdezi a LocalNetworkGateway|
|/ localnetworkgateways/írása|Létrehoz vagy frissít egy meglévő virtuális|
|/localnetworkgateways/DELETE|LocalNetworkGateway törlése|
|/trafficManagerProfiles/Read|Hello Traffic Manager-profil konfigurációjának beolvasása. Ez magában foglalja a DNS-beállítások, forgalom útválasztásához tartozó beállításokat, végpontmonitoring beállításai, és a Traffic Manager-profil által irányított végpontok hello listáját.|
|/ trafficManagerProfiles/írása|Traffic Manager-profil létrehozása, vagy módosíthatja egy meglévő Traffic Manager-profil hello konfigurációját. Ez magában foglalja a engedélyezésére, vagy egy profil letiltása folyamatban van, és módosítja a DNS-beállítások, a forgalom útválasztásához tartozó beállításokat vagy a végpontmonitoring beállításai. Hello Traffic Manager-profil által kezelendő végpontok hozzáadására, eltávolítva, engedélyezve van vagy le van tiltva.|
|/trafficManagerProfiles/DELETE|Hello Traffic Manager-profil törlése. Hello Traffic Manager-profil társított összes beállítást elvész, és hello-profil már nem lehet tooroute forgalom használt.|
|/dnsoperationstatuses/Read|A DNS-művelet állapotának beolvasása |
|/Operations/Read|Rendelkezésre álló műveletek beolvasása|
|/expressRouteCircuits/Read|Az Express route-körnek beolvasása|
|/ expressRouteCircuits/írása|Létrehoz vagy frissít egy meglévő Express route-körnek|
|/expressRouteCircuits/DELETE|Az Express route-körnek törlése|
|/expressRouteCircuits/stats/Read|Az Express route-körnek Stat beolvasása|
|/expressRouteCircuits/peerings/Read|Lekérdezi az Express route-körnek társviszony-létesítés|
|/expressRouteCircuits/peerings/Write|Létrehoz vagy frissít egy meglévő Express route-körnek Társviszony|
|/expressRouteCircuits/peerings/DELETE|Az Express route-körnek Társviszony törlése|
|/expressRouteCircuits/peerings/arpTables/Action|Az Express route-körnek társviszony-létesítés ArpTable beolvasása|
|/expressRouteCircuits/peerings/routeTables/Action|Az Express route-körnek társviszony-létesítés Migrálták beolvasása|
|/expressRouteCircuits/társviszony /<br>routeTablesSummary/művelet|Az Express route-körnek társviszony-létesítés Migrálták összegzését beolvasása|
|/expressRouteCircuits/peerings/stats/Read|Lekérdezi az Express route-körnek társviszony-létesítés statisztika|
|/expressRouteCircuits/authorizations/Read|Lekérdezi az Express route-körnek engedély|
|/expressRouteCircuits/authorizations/Write|Létrehozza vagy frissíti a meglévő Express route-körnek engedély|
|/expressRouteCircuits/authorizations/DELETE|Törli az Express route-körnek engedélyezési|
|/Connections/Read|Lekérdezi a ConnectionType(hypernet/routebased)|
|/ kapcsolatok/írása|Létrehoz vagy frissít egy meglévő ConnectionType(hypernet/routebased)|
|/Connections/DELETE|Törli a ConnectionType(hypernet/routebased)|
|/Connections/sharedKey/Read|Lekérdezi a ConnectionType(hypernet/routebased) SharedKey|
|/Connections/sharedKey/Write|Létrehoz vagy frissít egy meglévő ConnectionType(hypernet/routebased) SharedKey|
|/networkSecurityGroups/Read|Hálózati biztonsági csoport definíciójának beolvasása|
|/ biztonsági csoportok/írása|Hálózati biztonsági csoport létrehozása vagy meglévő hálózati biztonsági csoport frissítése|
|/networkSecurityGroups/DELETE|A hálózati biztonsági csoport törlése|
|/networkSecurityGroups/JOIN/Action|Hálózati biztonsági csoport illesztése|
|/networkSecurityGroups/defaultSecurityRules/Read|Alapértelmezett biztonsági szabály definíciójának beolvasása|
|/networkSecurityGroups/securityRules/Read|Biztonsági szabály definíciójának beolvasása|
|/networkSecurityGroups/securityRules/Write|A szabály létrehozása vagy meglévő biztonsági szabály frissítése|
|/networkSecurityGroups/securityRules/DELETE|A biztonsági szabály törlése|
|/applicationGateways/Read|Alkalmazásátjáró beolvasása|
|/ applicationGateways/írása|Alkalmazásátjáró létrehozása vagy meglévő Alkalmazásátjáró frissítése|
|/applicationGateways/DELETE|Alkalmazásátjáró törlése|
|/applicationGateways/backendhealth/Action|Az átjáró háttér állapotának beolvasása|
|/applicationGateways/Start/Action|Alkalmazásátjáró kezdődik|
|/applicationGateways/STOP/Action|Leállítja az Alkalmazásátjáró|
|/applicationGateways/backendAddressPools/JOIN/Action|Egy alkalmazás Alkalmazásátjáró háttércímkészletének illesztése|
|/routeTables/Read|Útvonaltábla-definíció beolvasása|
|/ routeTables/írása|Útvonaltábla létrehozása vagy meglévő útvonaltábla frissítése|
|/routeTables/DELETE|Útvonaltábla-definíció törlése|
|/routeTables/JOIN/Action|Egy útválasztási táblázatot illesztése|
|/routeTables/routes/Read|Útvonal definíciójának beolvasása|
|/routeTables/routes/Write|Új útvonal létrehozása vagy meglévő útvonal frissítése|
|/routeTables/routes/DELETE|Útvonal-definíció törlése|
|/Locations/operationResults/Read|Aszinkrón POST vagy DELETE művelet eredményének beolvasása|
|/Locations/checkDnsNameAvailability/Read|Ellenőrzi, ha a DNS-címke található hello megadott helyre|
|/Locations/usages/Read|Hello erőforrások a szoftverhasználati mérési adatok beolvasása|
|/Locations/Operations/Read|Egy aszinkron művelet állapotát jelző művelet erőforrás beolvasása|

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

| Művelet | Leírás |
|---|---|
|/ regisztrációs/művelet|Hello előfizetés hello NotifciationHubs erőforrás-szolgáltató regisztrálása, és lehetővé teszi, hogy a névterek és az NotificationHubs hello létrehozása|
|/ CheckNamespaceAvailability/művelet|Ellenőrzi, függetlenül attól, egy adott Namespace erőforrásnév hello NotificationHub szolgáltatás elérhető.|
|Névterek/írása|Namespace erőforrás létrehozása és frissítése a tulajdonságait. Címkék és a hello Namespace állapota nem frissíthető hello tulajdonságait.|
|Névterek/olvasása|Hello listáját Namespace erőforrás leírása|
|Vagy névterek/törlése|Namespace erőforrás törlése|
|/ Névterek/authorizationRules/művelet|Itt olvashatók névterek az engedélyezési szabályok leírás hello listája.|
|/ Névterek/CheckNotificationHubAvailability/művelet|Adott értesítésiközpont-név elérhetőségének ellenőrzése egy névtérben.|
|Névterek/authorizationRules/írása|A Namespace szintű engedélyezési szabályok létrehozása és frissítése a tulajdonságait. hello engedélyezési szabályok hozzáférési jogosultságokat, hello elsődleges és másodlagos kulcsok lehet frissíteni.|
|Névterek/authorizationRules/olvasása|Itt olvashatók névterek az engedélyezési szabályok leírás hello listája.|
|Vagy névterek/authorizationRules/törlése|Namespace engedélyezési szabály törlése. hello alapértelmezett Namespace engedélyezési szabály nem törölhető. |
|/ Névterek/authorizationRules/listkeys/művelet|Hello kapcsolati karakterlánc toohello Namespace beolvasása|
|/ Névterek/authorizationRules/regenerateKeys/művelet|Namespace engedélyezési szabály újragenerálja elsődleges vagy másodlagos kulcs, adjon meg hello toobe szükséges kulcs újragenerálása|
|Névterek/NotificationHubs/írása|Hozzon létre egy értesítési központot, és a tulajdonságok frissítése. A Tulajdonságok főként PNS hitelesítő adatok közé tartozik. Az engedélyezési szabályok és a TTL-t|
|Névterek/NotificationHubs/olvasása|Az értesítésiközpont-erőforrások leírásai listájának beolvasása|
|Vagy névterek/NotificationHubs/törlése|Értesítési központ erőforrás törlése|
|/ Névterek/NotificationHubs/authorizationRules/művelet|Hello listájának Notification Hub az engedélyezési szabályok|
|/ Névterek/NotificationHubs/pnsCredentials/művelet|Minden értesítési központ PNS hitelesítő adatainak lekérése. Ez magában foglalja a wns-ből, a MPNS, a APNS, a GCM és a Baidu hitelesítő adatokat|
|/ Névterek/NotificationHubs/debugSend/művelet|Teszt leküldéses értesítés küldése.|
|Névterek/NotificationHubs/metricDefinitions/olvasása|Namespace metrikák erőforrás leírása listájának beolvasása|
|/Namespaces/NotificationHubs /<br>authorizationRules írása|Értesítési központ engedélyezési szabályok létrehozása és frissítése a tulajdonságait. hello engedélyezési szabályok hozzáférési jogosultságokat, hello elsődleges és másodlagos kulcsok lehet frissíteni.|
|/Namespaces/NotificationHubs /<br>authorizationRules olvasása|Hello listájának Notification Hub az engedélyezési szabályok|
|/Namespaces/NotificationHubs /<br>authorizationRules vagy törlése|Értesítésiközpont-engedélyezési szabályok törlése|
|/Namespaces/NotificationHubs /<br>authorizationRules/listkeys/művelet|Hello kapcsolati karakterlánc toohello értesítési központ beolvasása|
|/Namespaces/NotificationHubs /<br>authorizationRules/regenerateKeys/művelet|Értesítési központ engedélyezési szabály újragenerálja elsődleges vagy másodlagos kulcs, adjon meg hello toobe szükséges kulcs újragenerálása|

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

| Művelet | Leírás |
|---|---|
|/ regisztrációs/művelet|Egy előfizetés tooa erőforrás-szolgáltató regisztrálása.|
|/linkTargets/Read|Felsorolja a meglévő fiókokat, amelyek nem kapcsolódnak az Azure-előfizetéssel. toolink az Azure-előfizetés tooa munkaterület, használja a felhasználói azonosító a művelet hello felhasználói azonosító tulajdonságában hello munkaterület létrehozása művelet adott vissza.|
|/ munkaterületek/írása|Új munkaterület vagy hivatkozások tooan meglévő munkaterület hozható létre, adja meg a hello ügyfél azonosítója hello meglévő munkaterületről.|
|/Workspaces/Read|Lekérdezi a meglévő-munkaterülettel|
|/Workspaces/DELETE|A munkaterület törlése. Ha hello munkaterület csatolt tooan meglévő munkaterületen, majd a létrehozáskor hello munkaterület csatolt toois nem törölve lett.|
|/Workspaces/generateregistrationcertificate/Action|Hello munkaterület regisztrációs tanúsítványt hoz létre. Ez a tanúsítvány használt tooconnect Microsoft System Center Operation Manager toohello munkaterület.|
|/Workspaces/sharedKeys/Action|Megosztott hello kulcsok hello munkaterület kéri le. Ezek a kulcsok használt tooconnect Microsoft Operational Insights ügynökök toohello munkaterületen.|
|/Workspaces/Search/Action|Keresési lekérdezés végrehajtása|
|/Workspaces/Datasources/Read|Adatforrások beolvasása az adott munkaterületen.|
|/Workspaces/Datasources/Write|Az adott munkaterületen Adatforrás létrehozása/frissítése.|
|/Workspaces/Datasources/DELETE|Törölje az adott munkaterületen adatforrásokhoz.|
|/Workspaces/managementGroups/Read|System Center Operations Manager felügyeleti csoportok csatlakoztatott toothis munkaterületen hello nevét és a metaadatok beolvasása.|
|/Workspaces/Schema/Read|Lekérdezi a hello keresési séma hello munkaterülethez.  Keresési séma kitett hello mezőket és a típusukat tartalmazza.|
|/Workspaces/usages/Read|Lekérdezi a használati adatok egy munkaterület hello mennyiségű hello munkaterület által beolvasott adatok.|
|/Workspaces/intelligencepacks/Read|Megjeleníti egy adott worksapce esetében az eszközintelligencia-csomagokat, és is megjeleníti, hogy hello csomag engedélyezett vagy letiltott adott munkaterület.|
|/Workspaces/intelligencepacks/enable/Action|Lehetővé teszi, hogy egy adott munkaterület egy intelligence Pack csomagot.|
|/Workspaces/intelligencepacks/disable/Action|Egy intelligence Pack csomagot egy adott munkaterület letiltja.|
|/Workspaces/sharedKeys/Read|Megosztott hello kulcsok hello munkaterület kéri le. Ezek a kulcsok használt tooconnect Microsoft Operational Insights ügynökök toohello munkaterületen.|
|/Workspaces/savedSearches/Read|Lekérdezi a mentett keresési lekérdezés|
|/Workspaces/savedSearches/Write|A mentett keresési lekérdezést hoz létre|
|/Workspaces/savedSearches/DELETE|Törli a mentett keresési lekérdezés|
|/Workspaces/storageinsightconfigs/Write|Létrehoz egy új tárolási konfigurációt. Ezek a beállítások egy hely a meglévő tárfiók használt toopull adatait.|
|/Workspaces/storageinsightconfigs/Read|Lekérdezi a tárolási konfiguráció.|
|/Workspaces/storageinsightconfigs/DELETE|A tárolási konfiguráció törlése. Ez a jövőben nem Microsoft Operational Insights adatainak olvasása hello tárfiókból.|
|/Workspaces/configurationScopes/Read|A konfigurációs hatókörben beolvasása|
|/Workspaces/configurationScopes/Write|Konfigurációs hatókör megadása|
|/Workspaces/configurationScopes/DELETE|A konfigurációs hatókörben törlése|

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

| Művelet | Leírás |
|---|---|
|/ regisztrációs/művelet|Egy előfizetés tooa erőforrás-szolgáltató regisztrálása.|
|/ megoldások írása|Új OMS-megoldás létrehozása|
|/Solutions/Read|Kilépés az OMS-megoldás beszerzése|
|/Solutions/DELETE|Törölje a meglévő OMS-megoldás|

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

| Művelet | Leírás |
|---|---|
|/ Tárolók/backupJobsExport/művelet|Exportálási feladat|
|Tárolók/írása|A tárlétrehozási művelettel vault típusú Azure-erőforrás hozható létre|
|Tárolók/olvasása|hello tároló Get művelet lekérdezi az Azure-erőforrás "tárolóban" típusú hello képviselő objektum|
|Vagy tárolók vagy törlése|a megadott Azure-erőforrás "tárolóban" típusú hello tároló törlése a művelet törli hello|
|Tárolók/refreshContainers/olvasása|Frissítheti hello tároló listáját|
|Tárolók/backupJobsExport/operationResults/olvasása|Beolvasása hello eredménye az Export feladat művelet.|
|Tárolók/backupOperationResults/olvasása|A Recovery Services-tárolóval kapcsolatos biztonsági mentés eredményét adja vissza.|
|Tárolók/monitoringAlerts/olvasása|Lekéri hello riasztások hello Recovery services-tároló.|
|/Vaults/monitoringAlerts / {uniqueAlertId} / olvasása|Lekérdezi a hello hello riasztás részletes adatait.|
|Tárolók/backupSecurityPIN/olvasása|Visszaadja a PIN-kód biztonsági tároló szolgáltatásról a helyreállításhoz.|
|/vaults/replicationEvents/Read|Bármely események olvasása|
|Tárolók/backupProtectableItems/olvasása|A védhető elemek listáját adja vissza.|
|/vaults/replicationFabrics/Read|A hálók olvasása|
|/vaults/replicationFabrics/Write|Létrehozni vagy frissíteni a háló|
|/vaults/replicationFabrics/Remove/Action|Távolítsa el a háló|
|/vaults/replicationFabrics/checkConsistency/Action|Hello háló konzisztenciájának ellenőrzése|
|/vaults/replicationFabrics/DELETE|A hálók törlése|
|/vaults/replicationFabrics/renewcertificate/Action||
|/vaults/replicationFabrics/deployProcessServerImage/Action|Lemezkép központi telepítése|
|/vaults/replicationFabrics/reassociateGateway/Action|Átjáró újbóli társítása|
|/ tárolók/replicationFabrics/replicationRecoveryServicesProviders /<br>Olvasás|Olvassa el az összes helyreállítási szolgáltatók|
|/ tárolók/replicationFabrics/replicationRecoveryServicesProviders /<br>Távolítsa el a/művelet|Recovery Services-szolgáltató eltávolítása|
|/ tárolók/replicationFabrics/replicationRecoveryServicesProviders /<br>törlése|Bármilyen helyreállítási szolgáltatók törlése|
|/ tárolók/replicationFabrics/replicationRecoveryServicesProviders /<br>refreshProvider/művelet|Frissítse a szolgáltatót|
|/vaults/replicationFabrics/replicationStorageClassifications/Read|A Tárhelybesorolások olvasása|
|/ tárolók/replicationFabrics/replicationStorageClassifications /<br>replicationStorageClassificationMappings olvasása|Olvassa el a tárolási besorolás leképezések|
|/ tárolók/replicationFabrics/replicationStorageClassifications /<br>replicationStorageClassificationMappings írása|Létrehozni vagy frissíteni a tárolási besorolás leképezések|
|/ tárolók/replicationFabrics/replicationStorageClassifications /<br>replicationStorageClassificationMappings vagy törlése|A tárolási besorolás leképezéseket törlése|
|/vaults/replicationFabrics/replicationvCenters/Read|Olvassa el a megfelelő feladat|
|/vaults/replicationFabrics/replicationvCenters/Write|A feladatok létrehozása vagy módosítása|
|/vaults/replicationFabrics/replicationvCenters/DELETE|Feladatok törlése|
|/vaults/replicationFabrics/replicationNetworks/Read|Olvassa el a hálózatok|
|/ tárolók/replicationFabrics/replicationNetworks /<br>replicationNetworkMappings olvasása|Olvassa el a hálózatok leképezését|
|/ tárolók/replicationFabrics/replicationNetworks /<br>replicationNetworkMappings írása|Hozzon létre vagy hálózati leképezések frissítése|
|/ tárolók/replicationFabrics/replicationNetworks /<br>replicationNetworkMappings vagy törlése|Hálózati leképezések törlése|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>Olvasás|Olvassa el a védelmi tárolókkal|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>discoverProtectableItem/művelet|Védhető objektum felderítése|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>írási|Létrehozni vagy frissíteni a védelmi tárolókkal|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>Távolítsa el a/művelet|Távolítsa el a védelmi tároló|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>switchprotection/művelet|Kapcsoló védelmi tároló|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>replicationProtectableItems olvasása|Bármely védhető elemek olvasása|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings olvasása|Olvassa el a védelmi tároló leképezések|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings írása|Létrehozni vagy frissíteni a védelmi tároló leképezések|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>a művelet/replicationProtectionContainerMappings/eltávolítása|Védelmitároló-leképezés eltávolítása|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings vagy törlése|A védelmi tároló leképezéseket törlése|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems olvasása|Minden védett elemek olvasása|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems írása|Minden védett cikkek létrehozása vagy frissítése|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems vagy törlése|Minden védett elemek törlése|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>a művelet/replicationProtectedItems/eltávolítása|Távolítsa el a védett elem|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/plannedFailover/művelet|Tervezett feladatátvétel|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/unplannedFailover/művelet|Feladatátvétel|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/testFailover/művelet|Feladatátvétel tesztelése|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/testFailoverCleanup/művelet|Teszt feladatátadás kitakarítását|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/failoverCommit/művelet|Feladatátvétel véglegesítésének|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/védelem-Újrabeállítási/művelet|Állítsa a védett elem|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/updateMobilityService/művelet|Frissítse a mobilitási szolgáltatás|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/repairReplication/művelet|Javítás replikáció|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/applyRecoveryPoint/művelet|Helyreállítási pont alkalmazása|
|/ tárolók/replicationFabrics/replicationProtectionContainers /<br>olvasási idő replicationProtectedItems/recoveryPoints|Olvassa el a replikációs helyreállítási pontot|
|/vaults/replicationPolicies/Read|Olvassa el az összes házirend|
|/vaults/replicationPolicies/Write|Bármely irányelveinek létrehozása vagy frissítése|
|/vaults/replicationPolicies/DELETE|A szabályzatoknak törlése|
|/vaults/replicationRecoveryPlans/Read|A helyreállítási tervek olvasása|
|/vaults/replicationRecoveryPlans/Write|A helyreállítási tervek létrehozása vagy módosítása|
|/vaults/replicationRecoveryPlans/DELETE|A helyreállítási terv törlése|
|/vaults/replicationRecoveryPlans/plannedFailover/Action|Tervezett feladatátvétel helyreállítási terv|
|/vaults/replicationRecoveryPlans/unplannedFailover/Action|Feladatátvételi helyreállítási terv|
|/vaults/replicationRecoveryPlans/testFailover/Action|Teszt feladatátvételi helyreállítási terv|
|/vaults/replicationRecoveryPlans/testFailoverCleanup/Action|Teszt feladatátadás kitakarítását helyreállítási terv|
|/vaults/replicationRecoveryPlans/failoverCommit/Action|Feladatátvétel véglegesítésének helyreállítási terv|
|/vaults/replicationRecoveryPlans/reProtect/Action|Állítsa a helyreállítási terv|
|Tárolók/extendedInformation/olvasása|hello információ kiterjesztett művelet lekérdezi egy objektum kiterjesztett Info hello Azure típusú erőforrást képviselő? tárolót?|
|Tárolók/extendedInformation/írása|hello információ kiterjesztett művelet lekérdezi egy objektum kiterjesztett Info hello Azure típusú erőforrást képviselő? tárolót?|
|Vagy tárolók/extendedInformation/törlése|hello információ kiterjesztett művelet lekérdezi egy objektum kiterjesztett Info hello Azure típusú erőforrást képviselő? tárolót?|
|Tárolók/backupManagementMetaData/olvasása|A Recovery Services-tároló biztonságimásolat-kezelési metaadatait adja vissza.|
|Tárolók/backupProtectionContainers/olvasása|Visszaadja az összes tároló tartozó toohello előfizetés|
|Tárolók/backupFabrics/operationResults/olvasása|Hello művelet állapotának beolvasása|
|Tárolók/backupFabrics/protectionContainers/olvasása|Minden regisztrált tárolókat ad vissza|
|/ Tárolók/backupFabrics/protectionContainers /<br>operationResults olvasása|Beolvassa a védelmi tárolón végrehajtott művelet eredményét.|
|/ Tárolók/backupFabrics/protectionContainers /<br>protectedItems olvasása|Értéket ad vissza objektumot hello védett elem részleteit|
|/ Tárolók/backupFabrics/protectionContainers /<br>protectedItems írása|Biztonsági mentési védett típusú elem létrehozása|
|/ Tárolók/backupFabrics/protectionContainers /<br>protectedItems vagy törlése|Törli a védett elem|
|/ Tárolók/backupFabrics/protectionContainers /<br>protectedItems, a biztonsági mentés/a művelet|Biztonsági másolat készítése egy védett elemről.|
|/ Tárolók/backupFabrics/protectionContainers /<br>olvasási idő protectedItems/operationResults|Beolvassa a védett elemeken végrehajtott művelet eredményét.|
|/ Tárolók/backupFabrics/protectionContainers /<br>olvasási idő protectedItems/operationStatus|Védett elemeken végrehajtott művelet hello állapota értéket ad vissza.|
|/ Tárolók/backupFabrics/protectionContainers /<br>olvasási idő protectedItems/recoveryPoints|Védett elemek helyreállítási pontjainak beolvasása.|
|/ Tárolók/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints /<br>visszaállítási/művelet|Védett elemek helyreállítási pontjainak visszaállítása.|
|/ Tárolók/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints /<br>provisionInstantItemRecovery/művelet|Védett elem rendelkezés azonnali elem helyreállítása|
|/ Tárolók/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints /<br>revokeInstantItemRecovery/művelet|Védett elem azonnali elem helyreállítása visszavonása|
|Tárolók/módjait/olvasása|Egy adott Recovery Services-tároló használati adatait adja vissza.|
|/vaults/usages/Read|Olvassa el a tároló módjait|
|Tárolók/tanúsítvány/írása|hello frissítés erőforrás tanúsítvány művelet frissíti a hello erőforrás/tárolói hitelesítő adatainak tanúsítványa.|
|Tárolók/tokenInfo/olvasása|Recovery Services-tároló információi token értéket ad vissza.|
|/vaults/replicationAlertSettings/Read|Olvassa el a riasztások beállításai|
|/vaults/replicationAlertSettings/Write|Minden riasztás beállításainak létrehozása vagy frissítése|
|Tárolók/backupOperations/olvasása|Visszaadja a biztonsági mentési művelet állapotának helyreállítási szolgáltatások tárolóban.|
|Tárolók/storageConfig/olvasása|Helyreállítási beolvasása-tárolási konfigurációját szolgáltatások tárolóban.|
|Tárolók/storageConfig/írása|Frissítések Tárkonfigurációt helyreállítási szolgáltatások tárolóban.|
|Tárolók/backupUsageSummaries/olvasása|Recovery Services beolvasása védett elemek és a védett kiszolgálók összesítések.|
|Tárolók/backupProtectedItems/olvasása|Minden védett elemek beolvasása hello listáját.|
|Tárolók/backupconfig/vaultconfig/olvasása|A helyreállításhoz adja vissza konfigurációs szolgáltatások tárolóban.|
|Tárolók/backupconfig/vaultconfig/írása|Frissítések konfigurációja helyreállítási szolgáltatások tárolóban.|
|Tárolók/registeredIdentities/írása|hello szolgáltatás tárolójának regisztrálni művelet lehet használt tooregister szolgáltatásban egy tárolót.|
|Tárolók/registeredIdentities/olvasása|hello beolvasása tárolók művelet alkalmazható erőforrás regisztrált hello tárolók beolvasása.|
|Vagy tárolók/registeredIdentities/törlése|hello tároló regisztrációját művelet lehet használt toounregister egy tárolót.|
|Tárolók/registeredIdentities/operationResults/olvasása|hello beolvasni a művelet eredménye műveletet is használt get hello műveleti állapotának és aszinkron módon kedvezőtlen hatással hello benyújtott művelet|
|/vaults/replicationJobs/Read|Olvassa el a megfelelő feladat|
|/vaults/replicationJobs/Cancel/Action|Megszakítása|
|/vaults/replicationJobs/restart/Action|Indítsa újra a feladatot|
|/vaults/replicationJobs/Resume/Action|Feladat folytatása|
|Tárolók/backupPolicies/olvasása|Visszaadja az összes védelmi házirend|
|Tárolók/backupPolicies/írása|Védelmi házirend létrehozása|
|Vagy tárolók/backupPolicies/törlése|Védelmi házirend törlése|
|Tárolók/backupPolicies/operationResults/olvasása|A szabályzatművelet eredményeinek beolvasása.|
|Tárolók/backupPolicies/operationStatus/olvasása|Házirend művelet állapotának beolvasása.|
|Tárolók/vaultTokens/olvasása|hello tároló Token művelet lehet használt tooget tároló Token tároló szintű háttérbeli műveletek.|
|Tárolók/monitoringConfigurations/notificationConfiguration/olvasása|Lekérdezi a hello helyreállítási tároló értesítési konfigurálása.|
|Tárolók/backupJobs/olvasása|Minden feladat objektumot ad vissza|
|/ Tárolók/backupJobs/Mégse/művelet|Hello feladat megszakítása|
|Tárolók/backupJobs/operationResults/olvasása|Beolvasása hello Job művelet eredménye.|
|/Locations/allocateStamp/Action|AllocateStamp egy szolgáltatás által használt belső művelet|
|/Locations/allocatedStamp/Read|A Lefoglalt bélyegző beolvasása egy belső művelet, melyet a szolgáltatás használ|

## <a name="microsoftrelay"></a>Microsoft.Relay

| Művelet | Leírás |
|---|---|
|/ checkNamespaceAvailability/művelet|A névtér az adott előfizetésben való elérhetőségének ellenőrzése.|
|/ regisztrációs/művelet|Hello előfizetés hello továbbítási erőforrás-szolgáltató regisztrálása, és lehetővé teszi a továbbítási források hello létrehozását|
|/ névterek/írása|Namespace erőforrás létrehozása és frissítése a tulajdonságait. Címkék és a hello Namespace állapota nem frissíthető hello tulajdonságait.|
|/Namespaces/Read|Hello listáját Namespace erőforrás leírása|
|/ névterek/törlése|Namespace erőforrás törlése|
|/Namespaces/authorizationRules/Write|A Namespace szintű engedélyezési szabályok létrehozása és frissítése a tulajdonságait. hello engedélyezési szabályok hozzáférési jogosultságokat, hello elsődleges és másodlagos kulcsok lehet frissíteni.|
|/Namespaces/authorizationRules/DELETE|Namespace engedélyezési szabály törlése. hello alapértelmezett Namespace engedélyezési szabály nem törölhető. |
|/Namespaces/authorizationRules/listkeys/Action|Hello kapcsolati karakterlánc toohello Namespace beolvasása|
|/Namespaces/HybridConnections/Write|Hozzon létre vagy frissítés HybridConnection tulajdonságai.|
|/Namespaces/HybridConnections/Read|Listájának HybridConnection erőforrás leírása|
|/Namespaces/HybridConnections/DELETE|A művelet toodelete HybridConnection erőforrás|
|/Namespaces/HybridConnections/authorizationRules/Write|HybridConnection engedélyezési szabályok létrehozása és frissítése a tulajdonságait. hello engedélyezési szabályok hozzáférési jogosultságokat, hello elsődleges és másodlagos kulcsok lehet frissíteni.|
|/Namespaces/HybridConnections/authorizationRules/DELETE|A művelet toodelete HybridConnection engedélyezési szabályok|
|/Namespaces/HybridConnections/authorizationRules/listkeys/Action|Hello kapcsolati karakterlánc tooHybridConnection beolvasása|
|/Namespaces/WcfRelays/Write|Hozzon létre vagy frissítés WcfRelay tulajdonságai.|
|/Namespaces/WcfRelays/Read|Listájának WcfRelay erőforrás leírása|
|/Namespaces/WcfRelays/DELETE|A művelet toodelete WcfRelay erőforrás|
|/Namespaces/WcfRelays/authorizationRules/Write|WcfRelay engedélyezési szabályok létrehozása és frissítése a tulajdonságait. hello engedélyezési szabályok hozzáférési jogosultságokat, hello elsődleges és másodlagos kulcsok lehet frissíteni.|
|/Namespaces/WcfRelays/authorizationRules/DELETE|A művelet toodelete WcfRelay engedélyezési szabályok|
|/Namespaces/WcfRelays/authorizationRules/listkeys/Action|Hello kapcsolati karakterlánc tooWcfRelay beolvasása|

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

| Művelet | Leírás |
|---|---|
|AvailabilityStatuses/olvasása|A megadott hatókör lekérdezi hello rendelkezésre állási állapotát hello összes erőforrása|
|AvailabilityStatuses/current/olvasása|A megadott erőforrás lekérdezi hello rendelkezésre állási állapotát hello|

## <a name="microsoftresources"></a>Microsoft.Resources

| Művelet | Leírás |
|---|---|
|/ checkResourceName/művelet|Hello erőforrásnév érvényességének ellenőrzése.|
|/Providers/Read|Hello szolgáltatók listájának lekérése.|
|/Subscriptions/Read|Beolvassa az előfizetések hello listáját.|
|/Subscriptions/operationresults/Read|Eredményt hello előfizetés műveletet.|
|/Subscriptions/Providers/Read|Beolvassa vagy listázza az erőforrás-szolgáltatókat.|
|/Subscriptions/tagNames/Read|Beolvassa vagy listázza az előfizetéscímkéket.|
|/Subscriptions/tagNames/Write|Előfizetéscímke hozzáadása.|
|/Subscriptions/tagNames/DELETE|Törli az előfizetéscímkét.|
|/Subscriptions/tagNames/tagValues/Read|Beolvassa vagy listázza az előfizetéscímkék értékeit.|
|/Subscriptions/tagNames/tagValues/Write|Hozzáadja az előfizetéscímke értékét.|
|/Subscriptions/tagNames/tagValues/DELETE|Törli az előfizetéscímke értékét.|
|/Subscriptions/Resources/Read|Beolvassa az előfizetéshez tartozó erőforrásokat.|
|/Subscriptions/resourceGroups/Read|Beolvassa vagy listázza az erőforráscsoportokat.|
|/Subscriptions/resourceGroups/Write|Létrehozza vagy frissíti az erőforráscsoportot.|
|/Subscriptions/resourceGroups/DELETE|Törli az erőforráscsoportot és az ahhoz tartozó összes erőforrást.|
|/Subscriptions/resourceGroups/moveResources/Action|Erőforrásokat helyez át az egyik erőforrás csoport tooanother.|
|/Subscriptions/resourceGroups/validateMoveResources/Action|Egy erőforrás csoport tooanother az erőforrások áthelyezésének ellenőrzése.|
|/Subscriptions/resourcegroups/Resources/Read|Hello erőforráscsoport hello erőforrások lekérése.|
|/Subscriptions/resourcegroups/Deployments/Read|Beolvassa vagy listázza az üzemelő példányokat.|
|/Subscriptions/resourcegroups/Deployments/Write|Létrehozza vagy frissíti az üzemelő példányt.|
|/Subscriptions/resourcegroups/Deployments/operationstatuses/Read|Beolvassa vagy listázza a központi telepítési művelet állapotok.|
|/Subscriptions/resourcegroups/Deployments/Operations/Read|Beolvassa vagy listázza az üzembe helyezési műveleteket.|
|/Subscriptions/Locations/Read|Lekérdezi a hello támogatott helyek listáját.|
|/Links/Read|Beolvassa vagy listázza az erőforrás-hivatkozásokat.|
|/ hivatkozások/írása|Létrehozza vagy frissíti az erőforrás-hivatkozást.|
|/Links/DELETE|Törli az erőforrás-hivatkozást.|
|/tenants/Read|Beolvassa a bérlők hello listáját.|
|/resources/Read|Szűrők alapján erőforrások hello listájának beolvasása.|
|/Deployments/Read|Beolvassa vagy listázza az üzemelő példányokat.|
|/ központi telepítések/írása|Létrehozza vagy frissíti az üzemelő példányt.|
|/Deployments/DELETE|Törli az üzemelő példányt.|
|/Deployments/Cancel/Action|Megszakítja az üzembe helyezést.|
|/Deployments/Validate/Action|Érvényesíti az üzemelő példányt.|
|/Deployments/Operations/Read|Beolvassa vagy listázza az üzembe helyezési műveleteket.|

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

| Művelet | Leírás |
|---|---|
|/jobcollections/Read|Feladat gyűjtőjének beolvasása sikertelen|
|/ feladatgyűjtemények/írása|Feladatgyűjtemények létrehozása és frissítése|
|/jobcollections/DELETE|Sikertelen feladat-gyűjtemény törlése.|
|/jobcollections/enable/Action|Lehetővé teszi, hogy feladat-gyűjtemény.|
|/jobcollections/disable/Action|Letiltja a feladat-gyűjtemény.|
|/jobcollections/Jobs/Read|Feladat beolvasása.|
|/jobcollections/Jobs/Write|Feladatok létrehozása és frissítése|
|/jobcollections/Jobs/DELETE|Törli a feladatot.|
|/jobcollections/Jobs/Run/Action|A feladat futtatásakor.|
|/jobcollections/Jobs/generateLogicAppDefinition/Action|Logic App-definíció előállítása egy Scheduler-feladat alapján.|
|/jobcollections/Jobs/jobhistories/Read|Lekérdezi a feladatelőzmények.|

## <a name="microsoftsearch"></a>Microsoft.Search

| Művelet | Leírás |
|---|---|
|/ regisztrációs/művelet|Hello előfizetés hello keresési erőforrás-szolgáltató regisztrálása, és lehetővé teszi a keresési szolgáltatások hello létrehozását.|
|/ checkNameAvailability/művelet|Hello szolgáltatásnév rendelkezésre állását ellenőrzi.|
|/ searchServices/írása|Létrehozza vagy frissíti a hello keresési szolgáltatást.|
|/searchServices/Read|Beolvassa a hello keresési szolgáltatást.|
|/searchServices/DELETE|Törli a hello keresési szolgáltatást.|
|/searchServices/Start/Action|Elindítja a hello keresési szolgáltatást.|
|/searchServices/STOP/Action|Hello keresési szolgáltatás leállítása.|
|/searchServices/listAdminKeys/Action|Hello adminisztrációs kulcsok beolvasása.|
|/searchServices/regenerateAdminKey/Action|Hello adminisztrátori kulcs újragenerálása.|
|/searchServices/createQueryKey/Action|Hello lekérdezési kulcsot hoz létre.|
|/searchServices/queryKey/Read|Hello lekérdezési kulcsok beolvasása.|
|/searchServices/queryKey/DELETE|Törli a hello lekérdezési kulcsot.|

## <a name="microsoftsecurity"></a>Microsoft.Security

| Művelet | Leírás |
|---|---|
|/jitNetworkAccessPolicies/Read|Lekérdezi a hello just-in-time hálózati hozzáférési házirendek|
|/ jitNetworkAccessPolicies/írása|Létrehoz egy új just-in-time hálózati hozzáférési házirendet, vagy frissít egy meglévő|
|/jitNetworkAccessPolicies/initiate/Action|Kezdeményezi a közvetlenül az időponthoz kötött hálózati hozzáférési házirend|
|/securitySolutionsReferenceData/Read|Hello biztonsági megoldások hivatkozási adatainak beolvasása|
|/securityStatuses/Read|Az Azure-erőforrások állapotát állapotok hello biztonsági beolvasása|
|/webApplicationFirewalls/Read|Webalkalmazási tűzfalak hello beolvasása|
|/ webApplicationFirewalls/írása|Létrehoz egy új webalkalmazási tűzfal vagy frissít egy meglévő|
|/webApplicationFirewalls/DELETE|Webalkalmazási tűzfal törlése|
|/securitySolutions/Read|Lekérdezi a hello biztonsági megoldások|
|/ securitySolutions/írása|Létrehoz egy új biztonsági megoldást, vagy frissít egy meglévő|
|/securitySolutions/DELETE|Olyan biztonsági megoldás törlése|
|/Tasks/Read|Lekérdezi az összes rendelkezésre álló biztonsági javaslatok|
|/Tasks/dismiss/Action|Biztonsági ajánlás olyan környezetekben elvetése|
|/Tasks/Activate/Action|Biztonsági ajánlás olyan környezetekben aktiválása|
|/Policies/Read|Lekérdezi a hello biztonsági házirend|
|/ házirendek/írása|Frissítések hello biztonsági házirend|
|/applicationWhitelistings/Read|Hello alkalmazás whitelistings beolvasása|
|/ applicationWhitelistings/írása|Létrehoz egy új alkalmazások engedélyezése vagy frissít egy meglévő|

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement

| Művelet | Leírás |
|---|---|
|/ subscriptions/írása|Létrehozza vagy frissíti az előfizetés|
|/ átjárók/írása|Létrehozza vagy frissíti az átjáró|
|/Gateways/DELETE|Átjáró törlése|
|/Gateways/Read|Lekérdezi egy átjáró|
|/Gateways/regenerateprofile/Action|Hello átjáró profil újragenerálása|
|/Gateways/upgradetolatest/Action|Frissítések hello átjáró toohello legújabb verziója|
|/ csomópontok/írása|létrehozza vagy frissíti a csomópont|
|/NODES/DELETE|A csomópont törlése|
|/NODES/Read|Lekérdezi a csomópont|
|/ munkamenetek/írása|Létrehozza vagy frissíti a munkamenet|
|/SESSIONS/Read|A munkamenet beolvasása|
|/SESSIONS/DELETE|Törli a munkamenetet|

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

| Művelet | Leírás |
|---|---|
|/ checkNameAvailability/művelet|A névtér az adott előfizetésben való elérhetőségének ellenőrzése.|
|/ regisztrációs/művelet|Hello előfizetés hello Szolgáltatásbusz erőforrás-szolgáltató regisztrálása, és lehetővé teszi a Szolgáltatásbusz-erőforrások hello létrehozását|
|/ névterek/írása|Namespace erőforrás létrehozása és frissítése a tulajdonságait. Címkék és a hello Namespace állapota nem frissíthető hello tulajdonságait.|
|/Namespaces/Read|Hello listáját Namespace erőforrás leírása|
|/ névterek/törlése|Namespace erőforrás törlése|
|/Namespaces/metricDefinitions/Read|Namespace metrikák erőforrás leírása listájának beolvasása|
|/Namespaces/authorizationRules/Write|A Namespace szintű engedélyezési szabályok létrehozása és frissítése a tulajdonságait. hello engedélyezési szabályok hozzáférési jogosultságokat, hello elsődleges és másodlagos kulcsok lehet frissíteni.|
|/Namespaces/authorizationRules/Read|Itt olvashatók névterek az engedélyezési szabályok leírás hello listája.|
|/Namespaces/authorizationRules/DELETE|Namespace engedélyezési szabály törlése. hello alapértelmezett Namespace engedélyezési szabály nem törölhető. |
|/Namespaces/authorizationRules/listkeys/Action|Hello kapcsolati karakterlánc toohello Namespace beolvasása|
|/Namespaces/authorizationRules/regenerateKeys/Action|Hello elsődleges vagy másodlagos kulcs toohello erőforrás újragenerálása|
|/Namespaces/diagnosticSettings/Read|Namespace diagnosztikai beállítások erőforrás leírása listájának beolvasása|
|/Namespaces/diagnosticSettings/Write|Namespace diagnosztikai beállítások erőforrás leírása listájának beolvasása|
|/Namespaces/Queues/Write|Hozzon létre vagy frissítés várólista-tulajdonságok.|
|/Namespaces/Queues/Read|Várólista erőforrás leírása listájának beolvasása|
|/Namespaces/Queues/DELETE|A művelet toodelete várólista erőforrás|
|/Namespaces/Queues/authorizationRules/Write|Várólista engedélyezési szabályok létrehozása és frissítése a tulajdonságait. hello engedélyezési szabályok hozzáférési jogosultságokat, hello elsődleges és másodlagos kulcsok lehet frissíteni.|
|/Namespaces/Queues/authorizationRules/Read| Hello listájának várólista az engedélyezési szabályok|
|/Namespaces/Queues/authorizationRules/DELETE|A művelet toodelete várólista engedélyezési szabályok|
|/Namespaces/Queues/authorizationRules/listkeys/Action|Hello kapcsolati karakterlánc tooQueue beolvasása|
|/Namespaces/Queues/authorizationRules/regenerateKeys/Action|Hello elsődleges vagy másodlagos kulcs toohello erőforrás újragenerálása|
|/Namespaces/logDefinitions/Read|Listájának Namespace naplók erőforrás leírása|
|/Namespaces/topics/Write|Hozzon létre vagy frissítés témakör tulajdonságai.|
|/Namespaces/topics/Read|A témakör erőforrás leírások listáját|
|/Namespaces/topics/DELETE|A művelet toodelete témakör erőforrás|
|/Namespaces/topics/authorizationRules/Write|A témakör az engedélyezési szabályok létrehozása és frissítése a tulajdonságait. hello engedélyezési szabályok hozzáférési jogosultságokat, hello elsődleges és másodlagos kulcsok lehet frissíteni.|
|/Namespaces/topics/authorizationRules/Read| A témakör az engedélyezési szabályok hello listájának|
|/Namespaces/topics/authorizationRules/DELETE|A művelet toodelete a témakör az engedélyezési szabályok|
|/Namespaces/topics/authorizationRules/listkeys/Action|Hello kapcsolati karakterlánc tooTopic beolvasása|
|/Namespaces/topics/authorizationRules/regenerateKeys/Action|Hello elsődleges vagy másodlagos kulcs toohello erőforrás újragenerálása|
|/Namespaces/topics/Subscriptions/Write|Hozzon létre vagy frissítés TopicSubscription tulajdonságai.|
|/Namespaces/topics/Subscriptions/Read|Listájának TopicSubscription erőforrás leírása|
|/Namespaces/topics/Subscriptions/DELETE|A művelet toodelete TopicSubscription erőforrás|
|/Namespaces/topics/Subscriptions/Rules/Write|Hozzon létre vagy frissítés szabály tulajdonságait.|
|/Namespaces/topics/Subscriptions/Rules/Read|Szabály erőforrás leírása listájának beolvasása|
|/Namespaces/topics/Subscriptions/Rules/DELETE|A művelet toodelete szabály erőforrás|

## <a name="microsoftsql"></a>Microsoft.Sql

| Művelet | Leírás |
|---|---|
|/Servers/Read|Az előfizetés egy erőforráscsoportot a kiszolgálók listáját adja vissza|
|/ kiszolgálók/írása|Hozzon létre egy új kiszolgálót vagy egy erőforráscsoportban található előfizetés a meglévő kiszolgáló tulajdonságainak módosítása|
|/Servers/DELETE|A kiszolgáló és a rajta található adatbázisok és a rugalmas készletek törlése|
|/Servers/import/Action|Hozzon létre egy új adatbázist hello kiszolgálón, és a séma és adatainak áttelepítését egy DacPac csomag telepítése|
|/Servers/Upgrade/Action|Új funkció érhető el a legújabb verziót hello engedélyezze és adja meg a adatbázisok kiadás átalakítási térkép|
|/Servers/VulnerabilityAssessmentScans/Action|A biztonsági rés server vizsgálat végrehajtása|
|/Servers/operationResults/Read|A művelet tootrack végrehajtási állapotát az alacsonyabb verzióra toohigher kiszolgáló frissítése|
|/Servers/operationResults/DELETE|Megszakítás kiszolgáló verzió frissítése folyamatban|
|/Servers/securityAlertPolicies/Read|Hello kiszolgáló threat detection házirend a megadott kiszolgálón konfigurált adatait beolvasása|
|/Servers/securityAlertPolicies/Write|Hello server fenyegetések észlelése egy adott kiszolgáló módosítása|
|/Servers/securityAlertPolicies/operationResults/Read|Hello kiszolgáló Fenyegetésészlelés házirend Set művelet eredményének beolvasása|
|/Servers/Administrators/Read|Rendszergazda kiszolgálóadatok beolvasása|
|/Servers/Administrators/Write|Létrehozni vagy frissíteni a kiszolgáló rendszergazdája|
|/Servers/Administrators/DELETE|Törölje a kiszolgáló rendszergazdája hello kiszolgálóról|
|/Servers/recoverableDatabases/Read|Ez a művelet a vész-helyreállítási élő adatbázis toorestore adatbázis toolast ismert helyes biztonsági mentési pontok szolgál. Hello Legutóbbi helyes biztonsági másolatot kapcsolatos információkat ad vissza, de nem az hello adatbázis ténylegesen visszaállítása.|
|/Servers/serviceObjectives/Read|A szolgáltatási szint célkitűzései (más néven teljesítmény rétegek) a megadott kiszolgálón elérhető listájának beolvasása|
|/Servers/firewallRules/Read|Kiszolgáló tűzfalszabály részletei beolvasása|
|/Servers/firewallRules/Write|Létrehozni vagy frissíteni server tűzfalszabályt, amely meghatározza, engedélyezett tooconnect toohello kiszolgáló IP-címtartomány|
|/Servers/firewallRules/DELETE|Törli a tűzfalszabályt hello kiszolgálóról|
|/Servers/administratorOperationResults/Read|Kiszolgálói rendszergazda a művelet eredménye beolvasása|
|/Servers/recommendedElasticPools/Read|Ajánlott rugalmas készletek tooreduce költség beolvasni vagy historica erőforrás-használat alapján a teljesítmény javítása|
|/Servers/recommendedElasticPools/Metrics/Read|Egy adott kiszolgálóhoz ajánlott rugalmas adatbáziskészletek metrikák beolvasása|
|/Servers/recommendedElasticPools/Databases/Read|Ajánlott rugalmas adatbáziskészlet megadott kiszolgáló be az új adatbázisok beolvasása|
|/Servers/elasticPools/Read|A megadott kiszolgálón rugalmas adatbáziskészlet részleteit beolvasása|
|/Servers/elasticPools/Write|Hozzon létre egy új vagy létező rugalmas adatbáziskészlet tulajdonságainak módosítása|
|/Servers/elasticPools/DELETE|A meglévő rugalmas készlet törlése|
|/Servers/elasticPools/operationResults/Read|Egy adott rugalmas adatbázis-készlet művelet részleteinek beolvasása|
|/Servers/elasticPools/Providers/Microsoft.Insights/<br>metricDefinitions olvasása|Térjen vissza a rugalmas adatbáziskészletek elérhető metrikák típusú|
|/Servers/elasticPools/Providers/Microsoft.Insights/<br>diagnosticSettings olvasása|Hello hello erőforrás diagnosztikai beállításának beolvasása|
|/Servers/elasticPools/Providers/Microsoft.Insights/<br>diagnosticSettings írása|Létrehozza vagy frissíti az hello hello erőforrás diagnosztikai beállításának|
|/Servers/elasticPools/Metrics/Read|Térjen vissza a rugalmas készlet Erőforrás kihasználtsága metrikák|
|/Servers/elasticPools/elasticPoolDatabaseActivity/Read|Tevékenységek és egy adott adatbázisnak, amely része a rugalmas adatbáziskészlet részleteinek beolvasása|
|/Servers/elasticPools/advisors/Read|Tanácsadók hello rugalmas készlet rendelkezésre álló listát ad vissza|
|/Servers/elasticPools/advisors/Write|Frissítés Automatikus végrehajtás az advisor szinten a rugalmas készlet állapotát.|
|/Servers/elasticPools/advisors/recommendedActions/Read|Javasolt művelet a megadott advisor hello rugalmas készlet listáját adja vissza|
|/Servers/elasticPools/advisors/recommendedActions/Write|Javasolt művelet hello rugalmas készlet hello alkalmazása|
|/Servers/elasticPools/elasticPoolActivity/Read|Tevékenységek és egy adott rugalmas adatbáziskészlet részleteinek beolvasása|
|/Servers/elasticPools/Databases/Read|Listáját és az adatbázisok rugalmas készlet a megadott kiszolgálón részét képező adatainak beolvasása|
|/Servers/auditingPolicies/Read|Hello alapértelmezett server tábla naplózás a megadott kiszolgálón konfigurált házirend részleteinek beolvasása|
|/Servers/auditingPolicies/Write|Egy adott kiszolgáló naplózásának hello alapértelmezett server tábla módosítása|
|/Servers/disasterRecoveryConfiguration/operationResults/Read|Katasztrófa utáni helyreállítás konfigurációs műveletet eredményt|
|/Servers/advisors/Read|Tanácsadók hello kiszolgáló rendelkezésre álló listát ad vissza|
|/Servers/advisors/Write|Frissítések automatikus-hajtható végre az advisor a kiszolgáló szintjén állapotát.|
|/Servers/advisors/recommendedActions/Read|Javasolt művelet a megadott advisor hello kiszolgáló listáját adja vissza|
|/Servers/advisors/recommendedActions/Write|Javasolt művelet hello kiszolgálón hello alkalmazása|
|/Servers/usages/Read|Térjen vissza a kiszolgáló DTU-kvótáról és aktuális DTU consuption hello kiszolgálón belül minden adatbázis|
|/Servers/elasticPoolEstimates/Read|Ehhez a kiszolgálóhoz már létrehozott rugalmas készlet becslések listáját adja vissza|
|/Servers/elasticPoolEstimates/Write|Hoz létre új rugalmas készletbecslés megadott adatbázisok listája|
|/Servers/auditingSettings/Read|Hello server blob a megadott kiszolgálón konfigurált naplórendet részleteinek beolvasása|
|/Servers/auditingSettings/Write|Módosítsa a hello server blobnaplózási funkció egy adott kiszolgálóhoz|
|/Servers/auditingSettings/operationResults/Read|Hello server blob naplózási házirend-beállítási művelet eredményének beolvasása|
|/Servers/backupLongTermRetentionVaults/Read|A művelet nem használt tooget egy biztonsági mentési hosszú távú megőrzési tárolóban. Hello tárolóban regisztrált toothis kiszolgálóval kapcsolatos információkat ad vissza.|
|/Servers/backupLongTermRetentionVaults/Write|A biztonsági mentési hosszú távú megőrzési tároló regisztrálása|
|/Servers/restorableDroppedDatabases/Read|Az adatbázisok listája, amelyek el lettek dobva a megadott kiszolgálón még mindig belüli adatmegőrzési beolvasása. Ez a művelet az adatbázisok és kapcsolódó metaadatok, például a törlés napját listáját adja vissza.|
|/Servers/Databases/Read|Az előfizetés egy erőforráscsoportot a kiszolgálók listáját adja vissza|
|/Servers/Databases/Write|Hozzon létre egy új kiszolgálót vagy egy erőforráscsoportban található előfizetés a meglévő kiszolgáló tulajdonságainak módosítása|
|/Servers/Databases/DELETE|A kiszolgáló és a rajta található adatbázisok és a rugalmas készletek törlése|
|/Servers/Databases/export/Action|Hozzon létre egy új adatbázist hello kiszolgálón, és a séma és adatainak áttelepítését egy DacPac csomag telepítése|
|/Servers/Databases/VulnerabilityAssessmentScans/Action|A biztonsági rés adatbázis vizsgálat hajtható végre.|
|/Servers/Databases/pause/Action|A DataWarehouse kiadás adatbázis felfüggesztése|
|/Servers/Databases/Resume/Action|A DataWarehouse kiadás adatbázis folytatása|
|/Servers/Databases/operationResults/Read|A művelet adatbázis hosszú ideig futó művelet, például a skála tootrack előrehaladását.|
|/Servers/Databases/replicationLinks/Read|Egy adott adatbázist létrehozni a replikációs hivatkozások visszatérési részletei|
|/Servers/Databases/replicationLinks/DELETE|Leáll hello replikációs kapcsolat kényszerített módon, és az esetleges adatvesztés|
|/Servers/Databases/replicationLinks/unlink/Action|Állítsa le a hello replikációs kapcsolat kényszerített módon vagy hello partnerrel való szinkronizálás után|
|/Servers/Databases/replicationLinks/Failover/Action|Ezt az adatbázist és a hello replikációs kapcsolat elsődleges és felhasználásának hello távoli elsődleges be egy másodlagos feladatátvételi összes módosítás hello elsődleges, a szinkronizálás után|
|/Servers/Databases/replicationLinks/forceFailoverAllowDataLoss/Action|Feladatátvétel azonnal esetleges adatvesztés, így a database hello replikációs kapcsolat elsődleges és felhasználásának hello távoli be elsődleges be egy másodlagos|
|/Servers/Databases/replicationLinks/updateReplicationMode/Action|Hivatkozás toosynchronous replikációs mód vagy aszinkron módú frissítése|
|/Servers/Databases/replicationLinks/operationResults/Read|A hosszú ideig futó műveletek az adatbázis-replikációs hivatkozások állapotának beolvasása|
|/Servers/Databases/dataMaskingPolicies/Read|Hello adatmaszkolási konfigurálva egy adott adatbázisnak a házirend részleteinek beolvasása|
|/Servers/Databases/dataMaskingPolicies/Write|Adatmaszkolási egy adott adatbázis-házirend módosítása|
|/Servers/Databases/dataMaskingPolicies/Rules/Read|Hello adatmaszkolási egy adott adatbázisnak konfigurált házirendszabály részleteinek beolvasása|
|/Servers/Databases/dataMaskingPolicies/Rules/Write|Adatok házirend szabály egy adott adatbázisnak a maszkolás módosítása|
|/Servers/Databases/securityAlertPolicies/Read|Egy adott adatbázisnak konfigurált szabályzat hello fenyegetés részleteit beolvasása|
|/Servers/Databases/securityAlertPolicies/Write|Egy adott adatbázisnak hello fenyegetések észlelése szabályzatának módosítása|
|/Servers/Databases/Providers/Microsoft.Insights/<br>metricDefinitions olvasása|Térjen vissza a típusú ítélt adatbázisokhoz|
|/Servers/Databases/Providers/Microsoft.Insights/<br>diagnosticSettings olvasása|Hello hello erőforrás diagnosztikai beállításának beolvasása|
|/Servers/Databases/Providers/Microsoft.Insights/<br>diagnosticSettings írása|Létrehozza vagy frissíti az hello hello erőforrás diagnosztikai beállításának|
|/Servers/Databases/Providers/Microsoft.Insights/<br>logDefinitions olvasása|Az adatbázisok hello naplók beolvasása|
|/Servers/Databases/topQueries/Read|Értéket ad vissza kijelölt időszakra összesített értéket a kijelölt lekérdezésre vonatkozó futásidejű statisztikák|
|/Servers/Databases/topQueries/queryText/Read|A kijelölt lekérdezés Azonosítóját hello Transact-SQL szöveget adja vissza|
|/Servers/Databases/topQueries/statistics/Read|Értéket ad vissza kijelölt időszakra összesített értéket a kijelölt lekérdezésre vonatkozó futásidejű statisztikák|
|/Servers/Databases/connectionPolicies/Read|Egy adott adatbázisnak konfigurált hello kapcsolat házirend részleteinek beolvasása|
|/Servers/Databases/connectionPolicies/Write|Módosítsa a kapcsolatkezelési házirendet az egy adott adatbázisnak|
|/Servers/Databases/Metrics/Read|Térjen vissza az adatbázist Erőforrás kihasználtsága metrikák|
|/Servers/Databases/auditRecords/Read|Hello adatbázis blob auditálási rekordok beolvasása|
|/Servers/Databases/transparentDataEncryption/Read|Állapot és átlátható titkosítási biztonsági szolgáltatást egy adott adatbázisnak részleteinek beolvasása|
|/Servers/Databases/transparentDataEncryption/Write|Engedélyezheti vagy tilthatja le egy adott adatbázisnak az átlátható adattitkosítás|
|/Servers/Databases/transparentDataEncryption/operationResults/Read|Állapot és átlátható titkosítási biztonsági szolgáltatást egy adott adatbázisnak részleteinek beolvasása|
|/Servers/Databases/auditingPolicies/Read|Hello tábla naplózási házirend egy adott adatbázisnak konfigurált részleteinek beolvasása|
|/Servers/Databases/auditingPolicies/Write|Hello tábla naplózási házirend egy adott adatbázis módosítása|
|/Servers/Databases/dataWarehouseQueries/Read|Hello adatok adatraktár terjesztési lekérdezés adatait a kijelölt lekérdezés Azonosítóját adja vissza.|
|/ kiszolgálók/adatbázisok/dataWarehouseQueries /<br>dataWarehouseQuerySteps olvasása|Beolvasása hello elosztott lekérdezési lépés információkhoz data warehouse lekérdezés kijelölt lépés|
|/Servers/Databases/serviceTierAdvisors/Read|Térjen javaslat adatbázis méretezésével felfelé vagy lefelé kapcsolatos lekérdezés végrehajtási statisztika tooimprove teljesítményadataira épülnek vagy költségeinek csökkentése|
|/Servers/Databases/advisors/Read|Tanácsadók hello adatbázis rendelkezésre álló listát ad vissza|
|/Servers/Databases/advisors/Write|Frissítés Automatikus végrehajtás az advisor adatbázis szinten állapotának.|
|/Servers/Databases/advisors/recommendedActions/Read|Javasolt művelet a megadott advisor hello adatbázis listáját adja vissza|
|/Servers/Databases/advisors/recommendedActions/Write|Javasolt művelet hello adatbázison hello alkalmazása|
|/Servers/Databases/usages/Read|Térjen vissza az adatbázis maximális méretét, amely elérhető és az adatok által elfoglalt jelenlegi mérete|
|/Servers/Databases/queryStore/Read|Hello adatbázis Lekérdezéstár beállításainak aktuális értékét adja vissza|
|/Servers/Databases/queryStore/Write|A Lekérdezéstár-beállítás a hello adatbázis frissítése|
|/Servers/Databases/auditingSettings/Read|Hello blob naplózási házirend egy adott adatbázisnak konfigurált részleteinek beolvasása|
|/Servers/Databases/auditingSettings/Write|Hello blob naplózási házirend egy adott adatbázis módosítása|
|/Servers/Databases/schemas/Tables/recommendedIndexes/Read|Egy adatbázishoz ajánlott index listájának beolvasása|
|/Servers/Databases/schemas/Tables/recommendedIndexes/Write|Alkalmazza az ajánlott index|
|/Servers/Databases/schemas/Tables/Columns/Read|Egy tábla oszlopainak listájának beolvasása|
|/Servers/Databases/missingindexes/Read|Térjen vissza az adatbázis indexek toocreate kapcsolatos javaslatok, illetve módosíthatja vagy rendelés tooimprove a lekérdezések teljesítményét törlése|
|/Servers/Databases/missingindexes/Write|Használja az adatbázishoz ajánlott index az adott adatbázishoz|
|/Servers/Databases/importExportOperationResults/Read|Adatbázis importálása adatait adja vissza, vagy exportálási művelet a tárfiókban található DacPac|
|/Servers/importExportOperationResults/Read|Adatbázis importálása műveletek hello lista visszaadásának storage-fiók a megadott kiszolgálón|

## <a name="microsoftstorage"></a>Microsoft.Storage

| Művelet | Leírás |
|---|---|
|/ regisztrációs/művelet|Hello előfizetés hello tárhely erőforrás-szolgáltató regisztrálása, és lehetővé teszi a storage-fiókok hello létrehozását.|
|/checknameavailability/Read|Ellenőrzi, hogy érvényes-e a fióknév, és nincs-e használatban.|
|/ storageAccounts/írása|Tárfiók létrehozása a megadott hello paraméterek vagy frissítés tulajdonságok vagy a címkék hello, vagy hozzáadja az egyéni tartomány hello a megadott tárfiók.|
|/storageAccounts/DELETE|Meglévő tárfiók törlése.|
|/storageAccounts/listkeys/Action|Beolvasása hello elérési kulcsainak hello megadott tárfiók.|
|/storageAccounts/regeneratekey/Action|Hello tárelérési kulcsok újragenerálása hello a megadott tárfiók.|
|/storageAccounts/Read|Beolvasása hello tárfiókok listájának vagy hello hello tulajdonságainak lekérdezi a megadott tárfiók.|
|/storageAccounts/listAccountSas/Action|Beolvasása hello fiók SAS-jogkivonat hello megadott tárfiók.|
|/storageAccounts/listServiceSas/Action|Tárolási szolgáltatás SAS-jogkivonat|
|/storageAccounts/Services/diagnosticSettings/Write|Tárolási fiók diagnosztikai beállításainak létrehozása/frissítése.|
|/skus/Read|Listák hello Microsoft.Storage által támogatott termékváltozat.|
|/usages/Read|Beolvasása hello korlát és jelenlegi kihasználtságuk értékének hello hello erőforrások megadott előfizetés|
|/Operations/Read|Szavazások hello egy aszinkron művelet állapotát.|
|/Locations/deleteVirtualNetworkOrSubnets/Action|Értesíti a Microsoft.Storage, hogy virtuális hálózathoz vagy alhálózathoz törlése folyamatban van|

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

| Művelet | Leírás |
|---|---|
|/managers/clearAlerts/Action|Törölje az Eszközkezelő hello tartozó összes hello riasztásokat.|
|/managers/getActivationKey/Action|Az Eszközkezelő hello aktiválási kulcs beszerzése.|
|/managers/regenerateActivationKey/Action|Az Eszközkezelő hello aktiválási kulcs újragenerálása.|
|/managers/regenarateRegistationCertificate/Action|Hello eszköz kezelők a regisztrációs tanúsítvány újbóli előállítása.|
|/managers/getEncryptionKey/Action|Hello Eszközkezelő a titkosítási kulcs beszerzése.|
|/managers/Read|Sorolja fel, vagy hello eszköz kezelői beolvasása|
|/managers/DELETE|Törli az eszköz kezelői hello|
|/ kezelők/írása|Létrehozása vagy frissítése hello eszköz kezelők|
|/managers/configureDevice/Action|Eszközök konfigurálása|
|/managers/listActivationKey/Action|Lekérdezi a StorSimple Device Manager hello hello használható aktiválási kulcs.|
|/managers/listPublicEncryptionKey/Action|Nyilvános titkosítási kulcsok egy StorSimple Device Manager listában.|
|/managers/listPrivateEncryptionKey/Action|Lekérdezi a személyes titkosítási kulcs egy StorSimple az Eszközkezelőben.|
|/managers/provisionCloudAppliance/Action|Hozzon létre egy új felhőalapú készülék.|
|Kezelői/írása|A tárlétrehozási művelettel vault típusú Azure-erőforrás hozható létre|
|Kezelői/olvasása|hello tároló Get művelet lekérdezi az Azure-erőforrás "tárolóban" típusú hello képviselő objektum|
|Vagy kezelők vagy törlése|a megadott Azure-erőforrás "tárolóban" típusú hello tároló törlése a művelet törli hello|
|/managers/storageAccountCredentials/Write|Hozzon létre vagy hello Tárfiók hitelesítő adatainak frissítése|
|/managers/storageAccountCredentials/Read|Sorolja fel, vagy hello Tárfiók hitelesítő adatainak beolvasása|
|/managers/storageAccountCredentials/DELETE|Hello Tárfiók hitelesítő adatainak törlése|
|/managers/storageAccountCredentials/listAccessKey/Action|A Tárfiók hitelesítő adatainak hozzáférési listázása|
|/managers/accessControlRecords/Read|Sorolja fel, vagy a hozzáférés-vezérlési rekordokat hello beolvasása|
|/managers/accessControlRecords/Write|Hello Access Control rekordok létrehozása vagy frissítése|
|/managers/accessControlRecords/DELETE|Hozzáférés-vezérlési rekordokat hello törlése|
|/managers/Metrics/Read|Sorolja fel, vagy hello metrikák beolvasása|
|/managers/bandwidthSettings/Read|Hello sávszélesség-beállítások (8000 sorozat csak)|
|/managers/bandwidthSettings/Write|Létrehoz egy új, vagy a sávszélesség-beállítások frissítése (8000 sorozat csak)|
|/managers/bandwidthSettings/DELETE|Törli a meglévő sávszélesség-beállítások (8000 sorozat csak)|
|Kezelői/extendedInformation/olvasása|hello információ kiterjesztett művelet lekérdezi egy objektum kiterjesztett Info hello Azure típusú erőforrást képviselő? tárolót?|
|Kezelői/extendedInformation/írása|hello információ kiterjesztett művelet lekérdezi egy objektum kiterjesztett Info hello Azure típusú erőforrást képviselő? tárolót?|
|Vagy kezelők/extendedInformation/törlése|hello információ kiterjesztett művelet lekérdezi egy objektum kiterjesztett Info hello Azure típusú erőforrást képviselő? tárolót?|
|/managers/Alerts/Read|Sorolja fel, vagy hello riasztások beolvasása|
|/managers/storageDomains/Read|Sorolja fel, vagy hello tárolási tartományok beolvasása|
|/managers/storageDomains/Write|Hozzon létre vagy hello tárolási tartományok frissítése|
|/managers/storageDomains/DELETE|Hello tárolási tartományok törlése|
|/managers/Devices/scanForUpdates/Action|Egy eszköz frissítések keresése.|
|/managers/Devices/download/Action|Egy eszköz letöltési frissítéseket.|
|/managers/Devices/Install/Action|Frissítések telepítése egy eszközön.|
|/managers/Devices/Read|Sorolja fel, vagy hello eszköz beolvasása|
|/managers/Devices/Write|Létrehozása vagy frissítése hello eszközök|
|/managers/Devices/DELETE|Hello eszközök törlése|
|/managers/Devices/Deactivate/Action|Egy eszköz inaktiválja.|
|/managers/Devices/publishSupportPackage/Action|Egy eszköz a Microsoft Support hibaelhárítási támogatási csomag közzététele.|
|/managers/Devices/Failover/Action|Hello eszköz feladatátvétele.|
|/managers/Devices/sendTestAlertEmail/Action|Teszt figyelmeztető e-mailt küldeni tooconfigured e-mailek címzettjeinek.|
|/managers/Devices/installUpdates/Action|Telepíti a frissítéseket a hello eszközök|
|/managers/Devices/listFailoverSets/Action|Lista hello feladatátvételi beállítja egy meglévő eszközt.|
|/managers/Devices/listFailoverTargets/Action|Feladatátvételi céljainak hello eszközök|
|/managers/Devices/publicEncryptionKey/Action|Az Eszközkezelő hello lista nyilvános titkosítási kulcs|
|/ kezelők/eszközök/hardwareComponentGroups /<br>Olvasás|Lista hello hardver összetevőcsoportok|
|/ kezelők/eszközök/hardwareComponentGroups /<br>changeControllerPowerState/művelet|Hardver összetevőcsoportok vezérlő power állapotának módosítása|
|/managers/Devices/Metrics/Read|Sorolja fel, vagy hello metrikák beolvasása|
|/managers/Devices/chapSettings/Write|Hello Chap beállításainak létrehozása vagy frissítése|
|/managers/Devices/chapSettings/Read|Sorolja fel, vagy hello Chap beállításainak beolvasása|
|/managers/Devices/chapSettings/DELETE|Törli a hello Chap beállításokat|
|/managers/Devices/backupScheduleGroups/Read|Sorolja fel, vagy hello biztonsági mentési ütemezés csoportok beolvasása|
|/managers/Devices/backupScheduleGroups/Write|Hello biztonsági mentési ütemezés csoportok létrehozása vagy frissítése|
|/managers/Devices/backupScheduleGroups/DELETE|Hello biztonsági mentési ütemezés csoportok törlése|
|/managers/Devices/updateSummary/Read|Sorolja fel, vagy frissítés összefoglalása hello beolvasása|
|/ kezelők/eszközök/migrationSourceConfigurations /<br>Importálás/művelet|Az áttelepítéshez forrás konfigurációk importálása|
|/ kezelők/eszközök/migrationSourceConfigurations /<br>startMigrationEstimate/művelet|Indítsa el a feladat tooestimate hello hello áttelepítési eljárás ideje alatt.|
|/ kezelők/eszközök/migrationSourceConfigurations /<br>startMigration/művelet|Forrás-konfigurációk használatával áttelepítés indítása|
|/ kezelők/eszközök/migrationSourceConfigurations /<br>confirmMigration/művelet|Megerősíti, hogy a sikeres áttelepítéshez és véglegesítheti.|
|/ kezelők/eszközök/migrationSourceConfigurations /<br>fetchMigrationEstimate/művelet|Hello állapot hello áttelepítési becslés feladat beolvasása.|
|/ kezelők/eszközök/migrationSourceConfigurations /<br>fetchMigrationStatus/művelet|Hello áttelepítési hello állapotának beolvasása.|
|/ kezelők/eszközök/migrationSourceConfigurations /<br>fetchConfirmMigrationStatus/művelet|Lehívási hello erősítse meg az áttelepítés állapotát.|
|/managers/Devices/alertSettings/Read|Sorolja fel, vagy hello riasztási beállításainak beolvasása|
|/managers/Devices/alertSettings/Write|Hello riasztási beállításainak létrehozása vagy frissítése|
|/managers/Devices/networkSettings/Read|Sorolja fel, vagy hello hálózati beállításainak beolvasása|
|/managers/Devices/networkSettings/Write|Létrehoz egy új, vagy a hálózati beállítások frissítése|
|/managers/Devices/Jobs/Read|Sorolja fel, vagy hello feladat beolvasása|
|/managers/Devices/Jobs/Cancel/Action|Egy futó feladat megszakítása|
|/managers/Devices/metricsDefinitions/Read|Sorolja fel, vagy hello metrikák meghatározások beolvasása|
|/managers/Devices/volumeContainers/Write|Létrehoz egy új vagy frissít Kötettárolók (8000 sorozat csak)|
|/managers/Devices/volumeContainers/Read|Hello Kötettárolók listában (8000 sorozat csak)|
|/managers/Devices/volumeContainers/DELETE|Törli a meglévő Kötettárolók (8000 sorozat csak)|
|/managers/Devices/volumeContainers/listEncryptionKeys/Action|Lista titkosítási kulcsokat a Kötettárolók|
|/managers/Devices/volumeContainers/rolloverEncryptionKey/Action|Kötettárolók a helyettesítő titkosítási kulcsok|
|/managers/Devices/volumeContainers/Metrics/Read|Lista hello metrikák|
|/managers/Devices/volumeContainers/Volumes/Read|Lista hello kötetek|
|/managers/Devices/volumeContainers/Volumes/Write|Létrehoz egy új vagy frissít kötetek|
|/managers/Devices/volumeContainers/Volumes/DELETE|Egy meglévő kötetek törlése|
|/managers/Devices/volumeContainers/Volumes/Metrics/Read|Lista hello metrikák|
|/managers/Devices/volumeContainers/Volumes/metricsDefinitions/Read|Lista hello metrikák definíciók|
|/managers/Devices/volumeContainers/metricsDefinitions/Read|Lista hello metrikák definíciók|
|/managers/Devices/iscsiservers/Read|Sorolja fel, vagy hello iSCSI kiszolgálók beolvasása|
|/managers/Devices/iscsiservers/Write|Hozzon létre vagy hello iSCSI kiszolgálók frissítése|
|/managers/Devices/iscsiservers/DELETE|Hello iSCSI kiszolgálók törlése|
|/managers/Devices/iscsiservers/Backup/Action|Az iSCSI-kiszolgálók biztonsági másolatok készítéséhez.|
|/managers/Devices/iscsiservers/Metrics/Read|Sorolja fel, vagy hello metrikák beolvasása|
|/managers/Devices/iscsiservers/Disks/Read|Sorolja fel, vagy hello lemezek beolvasása|
|/managers/Devices/iscsiservers/Disks/Write|Létrehozása vagy frissítése hello lemezek|
|/managers/Devices/iscsiservers/Disks/DELETE|Hello lemezek törlése|
|/managers/Devices/iscsiservers/Disks/Metrics/Read|Sorolja fel, vagy hello metrikák beolvasása|
|/managers/Devices/iscsiservers/Disks/metricsDefinitions/Read|Sorolja fel, vagy hello metrikák meghatározások beolvasása|
|/managers/Devices/iscsiservers/metricsDefinitions/Read|Sorolja fel, vagy hello metrikák meghatározások beolvasása|
|/managers/Devices/backups/Read|Sorolja fel, vagy a biztonságimásolat-készlet hello beolvasása|
|/managers/Devices/backups/DELETE|Törlések hello biztonságimásolat-készlet|
|/managers/Devices/backups/restore/Action|Állítsa vissza az összes hello kötet a biztonságimásolat-készletből.|
|/managers/Devices/backups/Elements/Clone/Action|Klónozni egy fájlmegosztás vagy kötet egy biztonsági mentési elem használatával.|
|/managers/Devices/backupPolicies/Write|Létrehoz egy új, vagy frissíti a biztonsági mentési házirendek (8000 sorozat csak)|
|/managers/Devices/backupPolicies/Read|Biztonsági házirendek lista hello (8000 sorozat csak)|
|/managers/Devices/backupPolicies/DELETE|Törli a meglévő biztonsági mentési házirendek (8000 sorozat csak)|
|/managers/Devices/backupPolicies/Backup/Action|Manuális biztonsági mentés toocreate igény szerinti érvénybe hello házirend által védett összes hello kötet biztonsági mentését.|
|/managers/Devices/backupPolicies/schedules/Write|Létrehoz egy új, vagy az ütemezések frissítése|
|/managers/Devices/backupPolicies/schedules/Read|Lista hello ütemezése|
|/managers/Devices/backupPolicies/schedules/DELETE|Egy meglévő ütemezés törlése|
|/managers/Devices/securitySettings/Update/Action|Hello biztonsági beállítások frissítése.|
|/managers/Devices/securitySettings/Read|Lista hello biztonsági beállítások|
|/ kezelők/eszközök/securitySettings /<br>syncRemoteManagementCertificate/művelet|A szinkronizálás hello távfelügyeleti tanúsítvány egy eszköz számára.|
|/managers/Devices/securitySettings/Write|Létrehoz egy új, vagy a biztonsági beállítások frissítése|
|/managers/Devices/fileservers/Read|Sorolja fel, vagy hello fájlkiszolgálók beolvasása|
|/managers/Devices/fileservers/Write|Létrehozása vagy frissítése hello fájlkiszolgálók|
|/managers/Devices/fileservers/DELETE|Hello fájlkiszolgálók törlése|
|/managers/Devices/fileservers/Backup/Action|Egy fájlkiszolgáló biztonsági másolatok készítéséhez.|
|/managers/Devices/fileservers/Metrics/Read|Sorolja fel, vagy hello metrikák beolvasása|
|/managers/Devices/fileservers/shares/Write|Létrehozni vagy frissíteni a megosztásokat hello|
|/managers/Devices/fileservers/shares/Read|Lekérdezi a hello megosztások vagy listája|
|/managers/Devices/fileservers/shares/DELETE|Hello megosztások törlése|
|/managers/Devices/fileservers/shares/Metrics/Read|Sorolja fel, vagy hello metrikák beolvasása|
|/managers/Devices/fileservers/shares/metricsDefinitions/Read|Sorolja fel, vagy hello metrikák meghatározások beolvasása|
|/managers/Devices/fileservers/metricsDefinitions/Read|Sorolja fel, vagy hello metrikák meghatározások beolvasása|
|/managers/Devices/timeSettings/Read|Sorolja fel, vagy hello idő beállításainak beolvasása|
|/managers/Devices/timeSettings/Write|Létrehoz egy új, vagy frissíti az idő beállítása|
|Kezelői/tanúsítvány/írása|hello frissítés erőforrás tanúsítvány művelet frissíti a hello erőforrás/tárolói hitelesítő adatainak tanúsítványa.|
|/managers/cloudApplianceConfigurations/Read|Lista hello felhő készülék támogatott konfigurációk|
|/managers/metricsDefinitions/Read|Sorolja fel, vagy hello metrikák meghatározások beolvasása|
|/managers/encryptionSettings/Read|Sorolja fel, vagy hello titkosítási beállításainak beolvasása|

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

| Művelet | Leírás |
|---|---|
|/streamingjobs/Start/Action|A Stream Analytics-feladat indítása|
|/streamingjobs/STOP/Action|A Stream Analytics-feladat leállítása|
|/ streamingjobs/Olvasás|Olvasási Stream Analytics-feladat|
|/ streamingjobs/írása|Írás a Stream Analytics-feladat|
|/ streamingjobs/törlése|A Stream Analytics-feladat törlése|
|/streamingjobs/Providers/Microsoft.Insights/metricDefinitions/Read|Az streamingjobs hello elérhető metrikák beolvasása|
|/streamingjobs/Providers/Microsoft.Insights/diagnosticSettings/Read|Olvassa el a diagnosztikai beállítást.|
|/streamingjobs/Providers/Microsoft.Insights/diagnosticSettings/Write|Diagnosztikai beállításának írási.|
|/streamingjobs/Providers/Microsoft.Insights/logDefinitions/Read|Az streamingjobs hello naplók beolvasása|
|/streamingjobs/Transformations/Read|Olvasási Stream Analytics feladat átalakítása|
|/streamingjobs/Transformations/Write|Írás a Stream Analytics feladat átalakítása|
|/streamingjobs/Transformations/DELETE|Törölje a Stream Analytics feladat átalakítása|
|/streamingjobs/Inputs/Read|Olvasási Stream Analytics feladat bemeneti|
|/streamingjobs/Inputs/Write|Írás Analytics-feladat adatfolyam-bemenet|
|/streamingjobs/Inputs/DELETE|Stream Analytics feladat bemeneti törlése|
|/streamingjobs/outputs/Read|Olvasási Stream Analytics Feladatkiemenetét|
|/streamingjobs/outputs/Write|Írás Analytics-feladat kimeneti adatfolyam|
|/streamingjobs/outputs/DELETE|Stream Analytics-feladat kimeneti törlése|

## <a name="microsoftsupport"></a>A Microsoft.Support

| Művelet | Leírás |
|---|---|
|/ regisztrációs/művelet|TooSupport erőforrás-szolgáltató regisztrálása|
|/supportTickets/Read|Támogatási jegy adatainak (beleértve az állapotot, súlyossága, kapcsolattartási adatai és kommunikációs) beolvasása, vagy hello előfizetésekhez tartozó támogatási jegyek listájának beolvasása.|
|/ supportTickets/írása|Létrehozza vagy frissíti egy támogatási jegy. Létrehozhat egy támogatási jegy műszaki, számlázási, kvóták és előfizetés-kezeléssel kapcsolatos problémákat. Súlyossága, kapcsolattartási adatai és a meglévő támogatási jegyek kommunikációs frissítheti.|

## <a name="microsoftweb"></a>Microsoft.Web

| Művelet | Leírás |
|---|---|
|/ unregister/művelet|Erőforrás-szolgáltató Microsoft.Web hello előfizetés regisztrációját.|
|/ ellenőrzése/művelet|Ellenőrzése.|
|/ regisztrációs/művelet|Erőforrás-szolgáltató Microsoft.Web hello előfizetés regisztrálása.|
|/ hostingEnvironments/Olvasás|Az App Service-környezetek hello tulajdonságainak beolvasása|
|/ hostingEnvironments/írása|Egy új App Service Environment-környezet létrehozása vagy meglévő ütemezés frissítése|
|/ hostingEnvironments/törlése|App Service-környezet törlése|
|/hostingEnvironments/reboot/Action|Indítsa újra a egy App Service-környezetben lévő összes gépen|
|/hostingenvironments/Resume/Action|Végezze el újra üzemeltetési környezetekben.|
|/hostingenvironments/suspend/Action|Felfüggesztheti üzemeltetési környezetekben.|
|/hostingenvironments/metricdefinitions/Read|Az üzemeltetési környezetek metrikai meghatározásainak beolvasása.|
|/hostingEnvironments/workerPools/Read|A Feldolgozókészleten egy App Service Environment-környezetben hello tulajdonságainak beolvasása|
|/hostingEnvironments/workerPools/Write|Hozzon létre egy új Feldolgozókészletek egy App Service Environment-környezetben, vagy egy meglévő frissítése|
|/hostingenvironments/workerpools/metricdefinitions/Read|Első üzemeltetési környezetekben Workerpools metrika definíciókat.|
|/hostingenvironments/workerpools/Metrics/Read|Első üzemeltetési környezetekben Workerpools metrikákat.|
|/hostingenvironments/workerpools/skus/Read|Első üzemeltetési környezetekben Workerpools SKU.|
|/hostingenvironments/workerpools/usages/Read|Első üzemeltetési környezetekben Workerpools módjait.|
|/hostingenvironments/Sites/Read|Első környezetek webalkalmazások üzemeltetéséhez.|
|/hostingenvironments/serverfarms/Read|Első üzemeltetési környezetekben App Service-csomagokról.|
|/hostingenvironments/usages/Read|Első üzemeltetési környezetekben is érvényesek.|
|/hostingenvironments/capacities/Read|Az üzemeltetési környezetek kapacitások beolvasása.|
|/hostingenvironments/Operations/Read|Első üzemeltetési környezetekben műveletek.|
|/hostingEnvironments/multiRolePools/Read|Egy előtér-címkészlet egy App Service Environment-környezetben hello tulajdonságainak beolvasása|
|/hostingEnvironments/multiRolePools/Write|Előtér-készlet létrehozása az App Service-környezetben, vagy egy meglévő frissítése|
|/hostingenvironments/multirolepools/metricdefinitions/Read|Az üzemeltetési környezetek többcélú készletek metrikai meghatározásainak beolvasása.|
|/hostingenvironments/multirolepools/Metrics/Read|Első üzemeltetési környezetekben többcélú készletek metrikákat.|
|/hostingenvironments/multirolepools/skus/Read|Első üzemeltetési környezetekben többcélú készletek SKU.|
|/hostingenvironments/multirolepools/usages/Read|Első üzemeltetési környezetekben többcélú készletek is érvényesek.|
|/hostingenvironments/Diagnostics/Read|Első üzemeltetési környezetekben diagnosztika.|
|/publishingusers/Read|Első közzététel számára.|
|/ publishingusers/írása|A frissítés közzététele a felhasználók.|
|/checknameavailability/Read|Ellenőrizze, hogy az erőforrásnév érhető el.|
|/ geoRegions/Olvasás|Földrajzi területek hello listájának beolvasása.|
|/ helyek/Olvasás|A webes alkalmazás hello tulajdonságainak beolvasása|
|/ helyek/írása|Új webalkalmazás létrehozása vagy meglévő pillanatkép frissítése|
|/ helyek/törlése|Egy már meglévő webalkalmazás törlése|
|/Sites/Backup/Action|Hozzon létre egy új webes alkalmazás biztonsági mentése|
|/Sites/publishxml/Action|Közzétételi profil xml egy webalkalmazást az beszerzése|
|/Sites/publish/Action|A webes alkalmazás közzététele|
|/Sites/restart/Action|A webalkalmazás újraindítása|
|/Sites/Start/Action|A webalkalmazás elindítása|
|/Sites/STOP/Action|A webalkalmazás leállítása|
|/Sites/slotsswap/Action|Felcserélni a webalkalmazás üzembe helyezési|
|/Sites/slotsdiffs/Action|Konfiguráció web app és az üzembe helyezési ponti közötti különbségek beolvasása|
|/Sites/applySlotConfig/Action|Webes alkalmazás tárolóhely konfigurációt a cél tárolóhely toohello aktuális webalkalmazásból|
|/Sites/resetSlotConfig/Action|Alaphelyzetbe állítja a webes alkalmazás konfigurálása|
|/Sites/Functions/Action|Webalkalmazások funkciók.|
|/Sites/listsyncfunctiontriggerstatus/Action|Lista szinkronizálási függvény eseményindító állapot webalkalmazások.|
|/Sites/networktrace/Action|Hálózati nyomkövetés webalkalmazások.|
|/Sites/newpassword/Action|ÚjJelszó webalkalmazások.|
|/Sites/Sync/Action|Szinkronizálási webalkalmazások.|
|/Sites/operationresults/Read|Web Apps művelet eredményt.|
|/Sites/webjobs/Read|Web Apps webjobs-feladatok beolvasása.|
|/ helyek/biztonsági mentési/olvasási|Web Apps biztonsági mentés beolvasása.|
|/Sites/Backup/Write|Web Apps másolat frissítéséhez.|
|/Sites/metricdefinitions/Read|Web Apps metrika meghatározások beolvasása.|
|/Sites/Metrics/Read|Web Apps metrikákat kaphat.|
|/Sites/continuouswebjobs/DELETE|Webes alkalmazások folyamatos webes feladatok törlése.|
|/Sites/continuouswebjobs/Read|Webes alkalmazások folyamatos webes feladatok beolvasása.|
|/Sites/continuouswebjobs/Start/Action|Indítsa el a webes alkalmazások folyamatos webes feladatok.|
|/Sites/continuouswebjobs/STOP/Action|Állítsa le a webes alkalmazások folyamatos webes feladatok.|
|/Sites/domainownershipidentifiers/Read|Megkapja a Web Apps tartományi tulajdonjoga azonosítókat.|
|/Sites/domainownershipidentifiers/Write|Frissítse a Web Apps tartományi tulajdonjoga azonosítók.|
|/Sites/premieraddons/DELETE|Törölje a Web Apps Premier bővítményei.|
|/Sites/premieraddons/Read|Web Apps Premier bővítményei beolvasása.|
|/Sites/premieraddons/Write|Frissítse a Web Apps Premier bővítményei.|
|/Sites/triggeredwebjobs/DELETE|Web Apps indított webjobs-feladatok törlése.|
|/Sites/triggeredwebjobs/Read|Web Apps indított webjobs-feladatok beolvasása.|
|/Sites/triggeredwebjobs/Run/Action|Web Apps indított webjobs-feladatok futtatásához.|
|/Sites/hostnamebindings/DELETE|Web Apps Állomásnévkötéseket törlése.|
|/Sites/hostnamebindings/Read|Web Apps Állomásnévkötéseket beolvasása.|
|/Sites/hostnamebindings/Write|Frissítse a Web Apps Állomásnévkötéseket.|
|/Sites/virtualnetworkconnections/DELETE|Törölje a Web Apps virtuális hálózati kapcsolatokat.|
|/Sites/virtualnetworkconnections/Read|Lekérni a Web Apps virtuális hálózati kapcsolatokat.|
|/Sites/virtualnetworkconnections/Write|Frissítse a Web Apps virtuális hálózati kapcsolatokat.|
|/Sites/virtualnetworkconnections/Gateways/Read|Web Apps virtuális hálózati kapcsolatok átjárók beolvasása.|
|/Sites/virtualnetworkconnections/Gateways/Write|Frissítse a Web Apps virtuális hálózati kapcsolatok átjárókat.|
|/Sites/publishxml/Read|Kérhető le a webes közzétételi XML.|
|/Sites/hybridconnectionrelays/Read|Web Apps hibrid kapcsolat továbbítók beolvasása.|
|/Sites/perfcounters/Read|Web Apps teljesítményszámlálók beolvasása.|
|/Sites/usages/Read|Web Apps módjait beolvasása.|
|/Sites/slots/Write|Webes alkalmazás új tárhely létrehozása vagy meglévő pillanatkép frissítése|
|/Sites/slots/DELETE|Törölje a meglévő Web App tárhelyek|
|/Sites/slots/Backup/Action|Hozzon létre új webalkalmazás biztonsági mentése.|
|/Sites/slots/publishxml/Action|Profil xml közzétételt a webalkalmazás beolvasása|
|/Sites/slots/publish/Action|Egy webes tárolóhelye közzététele|
|/Sites/slots/restart/Action|Indítsa újra a webes alkalmazás tárhely|
|/Sites/slots/Start/Action|Indítsa el a webes alkalmazás tárhely|
|/Sites/slots/STOP/Action|Állítsa le a webes alkalmazás tárhely|
|/Sites/slots/slotsswap/Action|Felcserélni a webalkalmazás üzembe helyezési|
|/Sites/slots/slotsdiffs/Action|Konfiguráció web app és az üzembe helyezési ponti közötti különbségek beolvasása|
|/Sites/slots/applySlotConfig/Action|A cél tárolóhely toohello aktuális tárhelyek web app tárolóhely beállításait alkalmazza.|
|/Sites/slots/resetSlotConfig/Action|Webes alkalmazás helyezési pont konfigurációjának visszaállítása|
|/Sites/slots/Read|A webalkalmazás üzembe helyezési pont hello tulajdonságainak beolvasása|
|/Sites/slots/newpassword/Action|ÚjJelszó Web Apps tárolóhelye.|
|/Sites/slots/Sync/Action|Szinkronizálási Web Apps tárolóhelye.|
|/Sites/slots/operationresults/Read|Webes alkalmazások üzembe helyezési ponti művelet eredményekhez juthat.|
|/Sites/slots/webjobs/Read|Webes alkalmazások üzembe helyezési ponti webjobs-feladatok beolvasása.|
|/Sites/slots/Backup/Write|Webes alkalmazások üzembe helyezési ponti másolat frissítéséhez.|
|/Sites/slots/metricdefinitions/Read|Webes alkalmazások üzembe helyezési ponti metrikai meghatározásainak beolvasása.|
|/Sites/slots/Metrics/Read|Webes alkalmazások üzembe helyezési ponti metrikákat kaphat.|
|/Sites/slots/continuouswebjobs/DELETE|Webes alkalmazások üzembe helyezési ponti folyamatos webes feladatok törlése.|
|/Sites/slots/continuouswebjobs/Read|Webes alkalmazások üzembe helyezési ponti folyamatos webes feladatok beolvasása.|
|/Sites/slots/continuouswebjobs/Start/Action|Indítsa el a webes alkalmazások üzembe helyezési ponti folyamatos webes feladatok.|
|/Sites/slots/continuouswebjobs/STOP/Action|Állítsa le a webes alkalmazások üzembe helyezési ponti folyamatos webes feladatok.|
|/Sites/slots/premieraddons/DELETE|Webes alkalmazások üzembe helyezési ponti Premier bővítményei törlése.|
|/Sites/slots/premieraddons/Read|Webes alkalmazások üzembe helyezési ponti Premier bővítményei beolvasása.|
|/Sites/slots/premieraddons/Write|Frissítse a webes alkalmazások üzembe helyezési ponti Premier bővítményei.|
|/Sites/slots/triggeredwebjobs/DELETE|Webes alkalmazások üzembe helyezési ponti indított webjobs-feladatok törlése.|
|/Sites/slots/triggeredwebjobs/Read|Webes alkalmazások üzembe helyezési ponti indított webjobs-feladatok beolvasása.|
|/Sites/slots/triggeredwebjobs/Run/Action|Webes alkalmazások üzembe helyezési ponti indított webjobs-feladatok futtatásához.|
|/Sites/slots/hostnamebindings/DELETE|Webes alkalmazások üzembe helyezési ponti Állomásnévkötéseket törlése.|
|/Sites/slots/hostnamebindings/Read|Webes alkalmazások üzembe helyezési ponti Állomásnévkötéseket beolvasása.|
|/Sites/slots/hostnamebindings/Write|Webes alkalmazások üzembe helyezési ponti Állomásnévkötéseket frissítése.|
|/Sites/slots/phplogging/Read|Webes alkalmazások üzembe helyezési ponti Phplogging beolvasása.|
|/Sites/slots/virtualnetworkconnections/DELETE|Törölje a webes alkalmazások üzembe helyezési ponti virtuális hálózati kapcsolatokat.|
|/Sites/slots/virtualnetworkconnections/Read|Lekérni a webes alkalmazások üzembe helyezési ponti virtuális hálózati kapcsolatokat.|
|/Sites/slots/virtualnetworkconnections/Write|Frissítse a webes alkalmazások üzembe helyezési ponti virtuális hálózati kapcsolatokat.|
|/Sites/slots/virtualnetworkconnections/Gateways/Write|Frissítse a webes alkalmazások üzembe helyezési ponti virtuális hálózati kapcsolatok átjárókat.|
|/Sites/slots/usages/Read|Webes alkalmazások üzembe helyezési ponti módjait beolvasása.|
|/Sites/slots/hybridconnection/DELETE|Webes alkalmazások üzembe helyezési ponti hibrid kapcsolat törlése.|
|/Sites/slots/hybridconnection/Read|Webes alkalmazások üzembe helyezési ponti hibrid kapcsolat lekérdezése.|
|/Sites/slots/hybridconnection/Write|Webes alkalmazások üzembe helyezési ponti hibrid kapcsolat frissítése.|
|/Sites/slots/config/Read|Webes alkalmazás a tárhely konfigurációs beállításainak beolvasása|
|/Sites/slots/config/List/Action|Webes alkalmazás a tárhely biztonsági bizalmas beállítások, például a hitelesítő adatokat, az alkalmazásbeállítások és az kapcsolati karakterláncok|
|/Sites/slots/config/Write|Webes alkalmazás a tárhely konfigurációs beállításainak frissítése|
|/Sites/slots/config/DELETE|Webes alkalmazások üzembe helyezési ponti konfiguráció törlése.|
|/Sites/slots/Instances/Read|Webes alkalmazások üzembe helyezési ponti-példányokat beszerezni.|
|/Sites/slots/Instances/Processes/Read|Webes alkalmazások üzembe helyezési ponti példányok folyamatok beolvasása.|
|/Sites/slots/Instances/Deployments/Read|Webes alkalmazások üzembe helyezési ponti példányok központi telepítések beolvasása.|
|/Sites/slots/sourcecontrols/Read|Webes alkalmazás a tárhely verziókezelő konfigurációs beállításainak beolvasása|
|/Sites/slots/sourcecontrols/Write|Webes alkalmazás a tárhely forrás a konfigurációs beállítások frissítése|
|/Sites/slots/sourcecontrols/DELETE|Webes alkalmazás a tárhely forrás a konfigurációs beállítások törlése|
|/Sites/slots/restore/Read|Webes alkalmazások üzembe helyezési ponti visszaállítási beolvasása.|
|/Sites/slots/analyzecustomhostname/Read|Első webes alkalmazások üzembe helyezési ponti elemezheti az egyéni állomásnevet.|
|/Sites/slots/backups/Read|A webalkalmazás tárolóhelyei biztonsági másolat hello tulajdonságainak beolvasása|
|/Sites/slots/backups/List/Action|Lista webes alkalmazások üzembe helyezési ponti biztonsági mentéseket.|
|/Sites/slots/backups/restore/Action|Állítsa vissza a webes alkalmazások üzembe helyezési ponti biztonsági mentéseket.|
|/Sites/slots/Deployments/DELETE|Törölje a webes alkalmazások üzembe helyezési ponti központi telepítések.|
|/Sites/slots/Deployments/Read|Webes alkalmazások üzembe helyezési ponti központi telepítések beolvasása.|
|/Sites/slots/Deployments/Write|Webes alkalmazások üzembe helyezési ponti központi telepítések frissítése.|
|/Sites/slots/Deployments/log/Read|Webes alkalmazások üzembe helyezési ponti központi telepítések napló beolvasása.|
|/Sites/hybridconnection/DELETE|Web Apps hibrid kapcsolat törlése.|
|/Sites/hybridconnection/Read|Web Apps a hibrid kapcsolat beolvasása.|
|/Sites/hybridconnection/Write|Web Apps hibrid kapcsolat frissítése.|
|/Sites/recommendationhistory/Read|Webes alkalmazások javaslat előzmények beolvasása.|
|/Sites/recommendations/Read|Webalkalmazás javaslatok listája az hello beolvasása.|
|/Sites/recommendations/disable/Action|Tiltsa le a Web Apps javaslatokat.|
|/Sites/config/Read|A webalkalmazás konfigurációs beállításainak beolvasása|
|/Sites/config/List/Action|Webes alkalmazás biztonsági bizalmas beállítások, például a hitelesítő adatokat, az alkalmazásbeállítások és az kapcsolati karakterláncok|
|/Sites/config/Write|Webes alkalmazás konfigurációs beállításainak frissítése|
|/Sites/config/DELETE|Webkonfiguráció alkalmazások törlése.|
|/Sites/Instances/Read|Web Apps-példányokat beszerezni.|
|/Sites/Instances/Processes/DELETE|Törölje a Web Apps példányok folyamatokat.|
|/Sites/Instances/Processes/Read|Web Apps példányok folyamatok beolvasása.|
|/Sites/Instances/Deployments/Read|Web Apps példányok központi telepítések beolvasása.|
|/Sites/sourcecontrols/Read|Webalkalmazás verziókövetésének konfigurációs beállításainak beolvasása|
|/Sites/sourcecontrols/Write|Webes alkalmazás forrás a konfigurációs beállítások frissítése|
|/Sites/sourcecontrols/DELETE|Webes alkalmazás forrás a konfigurációs beállítások törlése|
|/Sites/restore/Read|Web Apps visszaállítási beolvasása.|
|/Sites/analyzecustomhostname/Read|Elemezze az egyéni állomásnevet.|
|/Sites/backups/Read|A webalkalmazás biztonsági mentése hello tulajdonságainak beolvasása|
|/Sites/backups/List/Action|Lista Web Apps biztonsági mentéseket.|
|/Sites/backups/restore/Action|Állítsa vissza a Web Apps biztonsági mentéseket.|
|/Sites/snapshots/Read|Web Apps pillanatképek beolvasása.|
|/Sites/Functions/DELETE|Törölje a Web Apps funkciók.|
|/Sites/Functions/listsecrets/Action|Lista titkok Web Apps funkciók.|
|/Sites/Functions/Read|Web Apps funkciók beolvasása.|
|/Sites/Functions/Write|Frissítse a Web Apps funkciók.|
|/Sites/Deployments/DELETE|Webes alkalmazások központi telepítéseit törli.|
|/Sites/Deployments/Read|Webes alkalmazások központi telepítésének beolvasása.|
|/Sites/Deployments/Write|Webes alkalmazások központi telepítések frissítése.|
|/Sites/Deployments/log/Read|Webes alkalmazások központi telepítésének napló beolvasása.|
|/Sites/Diagnostics/Read|Web Apps diagnosztika beolvasása.|
|/Sites/Diagnostics/workerprocessrecycle/Read|Webes alkalmazások diagnosztika munkavégző folyamat újrahasznosítást beolvasása.|
|/Sites/Diagnostics/workeravailability/Read|Web Apps diagnosztika Workeravailability beolvasása.|
|/Sites/Diagnostics/runtimeavailability/Read|Web Apps diagnosztika futásidejű rendelkezésre beolvasása.|
|/Sites/Diagnostics/cpuanalysis/Read|Web Apps diagnosztika Cpuanalysis beolvasása.|
|/Sites/Diagnostics/servicehealth/Read|Webes alkalmazások diagnosztika szolgáltatás állapotának beolvasása.|
|/Sites/Diagnostics/frebanalysis/Read|Web Apps diagnosztika FREB elemzés beolvasása.|
|/availablestacks/Read|Rendelkezésre álló verem beolvasása.|
|/isusernameavailable/Read|Ellenőrizze, hogy a felhasználónév érhető el.|
|/Microsoft.Web/apiManagementAccounts/<br>API-k olvasása|API-k hello listájának beolvasása.|
|/Microsoft.Web/apiManagementAccounts/<br>API-k/írása|Adja hozzá egy új API-t, vagy frissítse a meglévőt.|
|/Microsoft.Web/apiManagementAccounts/<br>API-k vagy törlése|Törölje a meglévő Api.|
|/Microsoft.Web/apiManagementAccounts/<br>Olvasási idő API-k/kapcsolatok|Hello kapcsolatok listájának beolvasása.|
|/Microsoft.Web/apiManagementAccounts/<br>API-k/kapcsolatok/írása|Mentse az új kapcsolat, vagy frissítsen egy meglévőt.|
|/Microsoft.Web/apiManagementAccounts/<br>API-k vagy kapcsolatok/törlése|Törölje a meglévő kapcsolat.|
|/Microsoft.Web/apiManagementAccounts/<br>API-k/kapcsolatok/connectionAcls/Olvasás|Olvassa el a ConnectionAcls|
|/Microsoft.Web/apiManagementAccounts/<br>API-k/kapcsolatok/connectionAcls/írása|Hozzáadásakor vagy módosításakor ConnectionAcl|
|/Microsoft.Web/apiManagementAccounts/<br>API-k vagy kapcsolatok/connectionAcls/törlése|ConnectionAcl törlése|
|/Microsoft.Web/apiManagementAccounts/<br>API-k/connectionAcls/Olvasás|Olvassa el a ConnectionAcls API-hoz.|
|/Microsoft.Web/apiManagementAccounts/<br>API-k/apiAcls/Olvasás|Olvassa el a ConnectionAcls|
|/Microsoft.Web/apiManagementAccounts/<br>API-k/apiAcls/írása|Létrehozni vagy frissíteni az Api ACL-EK|
|/Microsoft.Web/apiManagementAccounts/<br>API-k vagy apiAcls/törlése|Hozzáférés-vezérlési listák Api törlése|
|/ serverfarms/Olvasás|Az App Service-csomag a hello tulajdonságainak beolvasása|
|/ serverfarms/írása|Egy új App Service-csomag létrehozása vagy meglévő pillanatkép frissítése|
|/ serverfarms/törlése|Egy meglévő App Service-csomag törlése|
|/serverfarms/restartSites/Action|Indítsa újra az összes webes alkalmazás az App Service-csomag|
|/serverfarms/operationresults/Read|Az alkalmazásszolgáltatási csomagok művelet eredményt.|
|/serverfarms/Capabilities/Read|Az alkalmazásszolgáltatási csomagok lehetőségek elérése.|
|/serverfarms/metricdefinitions/Read|Az alkalmazásszolgáltatási csomagok metrikai meghatározásainak beolvasása.|
|/serverfarms/Metrics/Read|Az alkalmazásszolgáltatási csomagok metrikákat kaphat.|
|/serverfarms/hybridconnectionplanlimits/Read|Az alkalmazásszolgáltatási csomagok hibrid kapcsolat terv korlátok beolvasása.|
|/serverfarms/virtualnetworkconnections/Read|Az alkalmazásszolgáltatási csomagok virtuális hálózati kapcsolatok lekérése.|
|/serverfarms/virtualnetworkconnections/routes/DELETE|Az App Service csomagokban virtuális hálózati kapcsolatok útvonalak törlése.|
|/serverfarms/virtualnetworkconnections/routes/Read|Az App Service csomagokban virtuális hálózati kapcsolatok útvonalak beolvasása.|
|/serverfarms/virtualnetworkconnections/routes/Write|Az alkalmazásszolgáltatási csomagok virtuális hálózati kapcsolatok az útvonalak frissítése.|
|/serverfarms/virtualnetworkconnections/Gateways/Write|Az App Service csomagokban virtuális hálózati kapcsolatok átjárók frissítése.|
|/serverfarms/firstpartyapps/Settings/DELETE|Törli az App Service csomagokban első entitás alkalmazásokra vonatkozó beállítások.|
|/serverfarms/firstpartyapps/Settings/Read|Az alkalmazásszolgáltatási csomagok első entitás alkalmazások beállításainak beolvasása.|
|/serverfarms/firstpartyapps/Settings/Write|Frissítés App Service-csomagok első entitás alkalmazásokra vonatkozó beállítások.|
|/serverfarms/Sites/Read|Az App Service csomagokban webes alkalmazások beszerzéséhez.|
|/serverfarms/workers/reboot/Action|Indítsa újra az App Service csomagokban munkavállalók.|
|/serverfarms/hybridconnectionrelays/Read|Az alkalmazásszolgáltatási csomagok hibrid kapcsolat továbbítók beolvasása.|
|/serverfarms/skus/Read|Az alkalmazásszolgáltatási csomagok termékváltozatok beolvasása.|
|/serverfarms/usages/Read|Az alkalmazásszolgáltatási csomagok módjait beolvasása.|
|/serverfarms/hybridconnectionnamespaces/relays/Sites/Read|Az App Service csomagokban hibrid kapcsolat névterek továbbítók webes alkalmazások beszerzéséhez.|
|/ishostnameavailable/Read|Ellenőrizze, hogy az állomásnév érhető el.|
|/ connectionGateways/Olvasás|Szerezze be a kapcsolat átjárók hello listát.|
|/ connectionGateways/írása|Létrehozza vagy frissíti a kapcsolat átjáró.|
|/ connectionGateways/törlése|Egy kapcsolat átjáró törlése.|
|/connectionGateways/JOIN/Action|Egy kapcsolat átjáró csatlakozik.|
|/classicmobileservices/Read|Klasszikus mobilszolgáltatások beolvasása.|
|/skus/Read|SKU beolvasása.|
|/ tanúsítvány/Olvasás|Tanúsítványok hello listájának beolvasása.|
|/ tanúsítvány/írása|Adja hozzá az új tanúsítványt, vagy frissítsen egy meglévőt.|
|/ tanúsítvány/törlése|Törölje a meglévő tanúsítványt.|
|/Operations/Read|Műveletek beolvasása.|
|/ javaslatok/Olvasás|Az előfizetések javaslatok listája az hello beolvasása.|
|/ishostingenvironmentnameavailable/Read|GET, ha üzemeltetési környezet neve érhető el.|
|/ apiManagementAccounts/Olvasás|ApiManagementAccounts hello listájának beolvasása.|
|/ apiManagementAccounts/írása|Adja hozzá egy új ApiManagementAccount vagy egy meglévő frissítése|
|/ apiManagementAccounts/törlése|Egy meglévő ApiManagementAccount törlése|
|/apiManagementAccounts/connectionAcls/Read|Szerezze be a kapcsolat ACL-ek hello listát.|
|/apiManagementAccounts/apiAcls/Read|Olvassa el a ConnectionAcls|
|/ kapcsolatok/Olvasás|Hello kapcsolatok listájának beolvasása.|
|/ kapcsolatok/írása|Létrehozza vagy frissíti a kapcsolatot.|
|/ kapcsolatok/törlése|Törli a kapcsolatot.|
|/Connections/JOIN/Action|A kapcsolat csatlakozik.|
|/Connections/confirmconsentcode/Action|Kapcsolatok hozzájárulási kód megerősítése.|
|/Connections/listconsentlinks/Action|Hozzájárulás hivatkozások kapcsolatokhoz.|
|/deploymentlocations/Read|Beolvasni a központi telepítési helyét.|
|/sourcecontrols/Read|Adatforrás-vezérlők beolvasása.|
|/ sourcecontrols/írása|Frissítse az adatforrás-vezérlők.|
|/managedhostingenvironments/Read|A felügyelt beolvasása üzemeltetési környezetekben.|
|/managedhostingenvironments/Sites/Read|Első felügyelt környezetek webalkalmazások üzemeltetéséhez.|
|/managedhostingenvironments/serverfarms/Read|A felügyelt beolvasása üzemeltetési környezetekben App Service-csomagokról.|
|/Locations/managedapis/Read|Helyek felügyelt API-k beolvasása.|
|/Locations/apioperations/Read|Helyek API műveleteinek beolvasása.|
|/Locations/connectiongatewayinstallations/Read|Helyek kapcsolat átjáró telepítések beolvasása.|
|/ listSitesAssignedToHostName/Olvasás|Telephelyhez toohostname nevének beolvasása.|

## <a name="next-steps"></a>Következő lépések

- Ismerje meg, hogyan túl[hozzon létre egy egyéni biztonsági szerepkört](role-based-access-control-custom-roles.md).

- Felülvizsgálati hello [beépített RBAC-szerepkörök](role-based-access-built-in-roles.md).

- Ismerje meg, hogyan férhetnek hozzá a toomanage hozzárendelések [felhasználó](role-based-access-control-manage-assignments.md) vagy [erőforrás](role-based-access-control-configure.md) 
