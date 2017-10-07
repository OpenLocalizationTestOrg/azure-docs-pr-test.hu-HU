---
title: Azure Automation-adatok aaaManaging |} Microsoft Docs
description: "Ez a cikk egy Azure Automation-környezet kezelése több témaköröket tartalmazza.  Jelenleg magában foglalja az adatok megőrzése és az Azure Automation vész-helyreállítási az Azure Automationben biztonsági mentéséről."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
ms.assetid: 2896f129-82e3-43ce-b9ee-a3860be0423a
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/02/201
ms.author: magoedte;bwren;sngun
ms.openlocfilehash: 46a164d864c4956c90ab689ca159fff6f6c08028
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-automation-data"></a>Azure Automation-adatok kezelése
Ez a cikk egy Azure Automation-környezet kezelése több témaköröket tartalmazza.

## <a name="data-retention"></a>Adatmegőrzés
Ha töröl egy erőforrást az Azure Automationben, véglegesen eltávolításuk előtt naplózási célokra 90 napig őrzi meg.  Ezt nem talál vagy hello erőforrás ebben az időszakban.  Ezt a házirendet törölnek tooan automation-fiókhoz tartozó tooresources is vonatkozik.

Azure Automation szolgáltatásbeli automatikusan törli, és véglegesen törli a 90 napnál régebbi feladatok.

a következő táblázat hello hello adatmegőrzési különböző erőforrások foglalja össze.

| Adatok | Szabályzat |
|:--- |:--- |
| Fiókok |Véglegesen törli a 90 nappal a felhasználó hello fiók törlése után. |
| Objektumok |Véglegesen törli a 90 nap után hello eszköz törlése a felhasználó, vagy számított 90 napon belül hello fiók, amely tárolja a hello eszköz törlése a felhasználó. |
| Modulok |Véglegesen törli a 90 nap után hello modul törlése a felhasználó, vagy számított 90 napon belül hello fiók, amely tárolja a hello modul törlése a felhasználó. |
| Runbookok |Véglegesen törli a 90 nap után hello erőforrás törlése a felhasználó, vagy számított 90 napon belül hello fiók, amely tárolja a hello erőforrás törlése a felhasználó. |
| Feladatok |Törölt és véglegesen eltávolított 90 nap után utolsó módosítás alatt. Ennek oka az lehet, miután hello feladat befejeződött, leállt, vagy fel van függesztve. |
| Csomópont-konfigurációk/MOF-fájlok |Régi csomópont-konfigurálás 90 nap után létrejön egy új csomópont-konfiguráció véglegesen törlődnek. |
| A DSC-csomópontok |Véglegesen törli a 90 nap után hello csomópont regisztrációját az Azure portál vagy hello használatával Automation-fiók [Unregister-AzureRMAutomationDscNode](https://msdn.microsoft.com/library/mt603500.aspx) a Windows PowerShell parancsmag. Csomópontok is véglegesen eltávolítja számított 90 napon belül, amely tárolja a felhasználó törlése hello csomópont hello fiók. |
| Csomópont-jelentések |Véglegesen törli a 90 nap után egy új jelentés készül az adott csomópont |

hello adatmegőrzési tooall felhasználók vonatkozik, és jelenleg nem szabható testre.

Azonban ha egy hosszabb ideig kell tooretain adatokat, továbbíthatja a runbook feladat naplók tooLog elemzés.  További információkért tekintse át [továbbítja az Azure Automation-feladat adatok tooOMS Naplóelemzési](automation-manage-send-joblogs-log-analytics.md).   

## <a name="backing-up-azure-automation"></a>Az Azure Automation biztonsági mentése
A Microsoft Azure automation-fiók törlésekor törlődnek hello fiók található összes objektumot, többek között a runbookok, modulok, konfigurációk, beállítások, feladatok és eszközök. hello objektumok nem állítható helyre, miután hello fiókot törölték.  Információ toobackup hello tartalmát az automation-fiók törlése előtt a következő hello is használhatja. 

### <a name="runbooks"></a>Runbookok
Exportálhatja a runbookok tooscript fájlokat hello Azure felügyeleti portálon vagy a hello [Get-AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) a Windows PowerShell parancsmag.  Ezek a parancsfájlok importálhatja egy másik automation-fiók leírtaknak megfelelően [létrehozása vagy importálása egy Runbook](https://msdn.microsoft.com/library/dn643637.aspx).

### <a name="integration-modules"></a>Integrációs modulok
Azure Automation integrációs modulok nem exportálható.  Gondoskodnia kell arról, hogy elérhetők, kívül hello automation-fiók.

### <a name="assets"></a>Objektumok
Nem lehet exportálni [eszközök](https://msdn.microsoft.com/library/dn939988.aspx) Azure Automation.  Hello Azure felügyeleti portált használja, meg kell vegye figyelembe változók, a hitelesítő adatokat, a tanúsítványok, a kapcsolatok és a ütemezések hello részleteit.  Ezután manuálisan kell létrehozni minden egy másik automation alkalmazásba importált runbookok által használt eszközök.

Használhat [Azure-parancsmagokkal](https://msdn.microsoft.com/library/dn690262.aspx) tooretrieve részleteit titkosítatlan eszközök, és menti azokat a jövőben hivatkozik, vagy ezzel egyenértékű eszközöket egy másik automation-fiókban létrehozni.

Titkosított változók vagy hello jelszó mező hitelesítőadat-parancsmagok használatával hello értéke nem olvasható be.  Ha nem tudja ezeket az értékeket, akkor egy runbookból hello segítségével helyreállíthatók [Get-AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx) és [Get-AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx) tevékenységeket.

Azure Automation tanúsítványok nem exportálható.  Gondoskodnia kell arról, hogy elérhetők-e a tanúsítványok Azure-on kívüli.

### <a name="dsc-configurations"></a>A DSC-konfigurációk
Exportálhatja a konfigurációk tooscript fájlokat hello Azure felügyeleti portálon vagy a hello [Export-AzureRmAutomationDscConfiguration](https://msdn.microsoft.com/library/mt603485.aspx) a Windows PowerShell parancsmag. Ezek a konfigurációk importálja, majd egy másik automation-fiókot használni.

## <a name="geo-replication-in-azure-automation"></a>Az Azure Automationben georeplikáció
Georeplikáció, Azure Automation-fiók, a standard készít biztonsági fiók adatok tooa különböző földrajzi régió a redundancia érdekében. Választhat egy elsődleges régióban, a fiók beállításakor, és ezután egy másodlagos régióban van hozzárendelve tooit automatikusan. hello másodlagos, másolt adatokra hello elsődleges régióban, folyamatosan frissített adatvesztés esetén.  

a következő táblázat hello hello érhető el az elsődleges és másodlagos régióban párosítása jeleníti meg.

| Elsődleges | Másodlagos |
| --- | --- |
| USA déli középső régiója |USA északi középső régiója |
| USA 2. keleti régiója |USA középső régiója |
| Nyugat-Európa |Észak-Európa |
| Délkelet-Ázsia |Kelet-Ázsia |
| Kelet-Japán |Nyugat-Japán |

Hello valószínű esemény, hogy egy elsődleges régió adatok nem vesztek el, a Microsoft megkísérel toorecover azt. Hello elsődleges adatok nem állítható helyre, ha elvégzi a földrajzi-feladatátvételt, és hello az érintett ügyfelek értesítést fog kapni erről a előfizetéssel.

