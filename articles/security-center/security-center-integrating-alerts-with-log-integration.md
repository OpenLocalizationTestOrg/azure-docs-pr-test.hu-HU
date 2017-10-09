---
title: "aaaIntegrating Azure Security Center riasztásait az Azure-ral jelentkezzen integrációs |} Microsoft Docs"
description: "Ez a cikk segít a Security Center riasztásait integrálása az Azure naplóelemzés integráció az első lépései."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: d2d088d3-d38d-47ff-a062-c78e0fd59226
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: terrylan
ms.openlocfilehash: 2649036ee990bf0f48fa0cb35c7495ac932c29ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrating-azure-security-center-alerts-with-azure-log-integration"></a>Azure Security Center riasztásait integrálása az Azure naplóelemzés integráció
Számos biztonsági műveletek és incidensekre adott reakciók csapatok támaszkodniuk biztonsági adatai és az esemény felügyeleti SIEM-megoldás a kiindulási pontjaként triaging és biztonsági riasztások kivizsgálásának hello. Az Azure napló integráció integrálható Azure Security Center riasztásait az SIEM-megoldás.

Azure naplóelemzés integrációs jelenleg a HP ArcSight Splunk és az IBM QRadar támogatja.

## <a name="what-logs-can-i-integrate"></a>Milyen naplókat is integrálhatja?
Azure kiterjedt naplózás minden szolgáltatás eredményez. Ezek a naplók kategóriába sorolni:

* **Ellenőrzés/Management-naplók** , amelyek biztosítják hello láthatósága Azure Resource Manager LÉTREHOZNI, UPDATE és DELETE műveletek. A vezérlő vezérlősík események illesztett hello Azure tevékenységi naplóit a
* **Az adatnaplók Vezérlősík** , amelyek biztosítják, hogy egy Azure-erőforrás használatakor kiváltott hello események láthatósága. Példa: hello Windows Eseménynapló, ahol letölthető biztonsági eseményadatok hello Eseménynapló használatával biztonsági csatorna. Adatok vezérlősík események (amely a virtuális gép vagy az Azure-szolgáltatások által előállított) által az Azure diagnosztikai naplók illesztett.

Azure naplóelemzés integrációs jelenleg hello integrációja támogatja:

* Az Azure virtuális gép naplók
* Az Azure felügyeleti naplók
* Azure Security Center riasztásait

## <a name="install-azure-log-integration"></a>Azure naplóelemzés integráció telepítése
Töltse le [Azure naplóelemzés integrációs](https://www.microsoft.com/download/details.aspx?id=53324).

hello Azure naplóelemzés integrációs szolgáltatás gyűjt telemetrikus adatokat hello számítógép, amelyen telepítve van.  Az összegyűjtött telemetrikus adatok van:

* Az Azure naplóelemzés integráció során bekövetkező kivételek
* Hello számáról, lekérdezések és a feldolgozott események metrikák
* Mely Azlog.exe kapcsolatos parancssori kapcsolók használt statisztikák

> [!NOTE]
> Ez a beállítás jelének kapcsolhatja ki telemetrikus adatok gyűjtését.
>
>

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Az Azure naplói és a Security Center riasztások integrálása
1. A parancssor megnyitása hello és **cd** történő **c:\Program Files\Microsoft Azure napló integrációs**.
2. Futtassa a hello **azlog createazureid** parancs toocreate egy [Azure Active Directory szolgáltatás egyszerű](../active-directory/active-directory-application-objects.md) hello Azure Active Directory (AD) a bérlők üzemeltető hello Azure-előfizetések.

    Az Azure bejelentkezési kéri.

   > [!NOTE]
   > Hello előfizetés hello előfizetés tulajdonosának vagy társadminisztrátorának kell lennie.
   >
   >

    Hitelesítési tooAzure az Azure AD segítségével történik.  Hello Azure AD identity arra, hogy hozzáférési tooread az Azure-előfizetések az Azure naplóelemzés integráció egyszerű szolgáltatás létrehozása hoz létre.
3. Futtatás hello **azlog engedélyezik <SubscriptionID>**  parancs tooassign olvasási hozzáférésének hello előfizetés toohello egyszerű szolgáltatásnév a 2. Ha nem adja meg a **SubscriptionID**, akkor hello szolgáltatás egyszerű hozzárendelt hello olvasó szerepkört tooall előfizetések toowhich rendelkezik hozzáféréssel.

   > [!NOTE]
   > Hello futtatásakor figyelmeztetések jelenhetnek **engedélyezik** parancs hello után azonnal **createazureid** parancsot. Néhány hello Azure AD-fiók létrehozásakor, és ha hello fiók használható közötti késés van. Ha hello futtatása után nagyjából 10 másodpercet vár **createazureid** parancs toorun hello **engedélyezik** parancsot, majd a figyelmeztetések nem szabadna megjelennie.
   >
   >
4. Ellenőrizze, hogy a következő mappák tooconfirm, amely a naplófájlok JSON hello hello vannak:

   * **c:\Users\azlog\AzureResourceManagerJson**
   * **c:\Users\azlog\AzureResourceManagerJsonLD**
5. Ellenőrizze a következő mappák tooconfirm, amely a Security Center riasztásait szerepel azokat hello:

   * **c:\Users\azlog\ AzureSecurityCenterJson**
   * **c:\Users\azlog\AzureSecurityCenterJsonLD**
6. Konfigurálja a hello SIEM fájl továbbító összekötő toohello megfelelő mappát. hello eljárás hello SIEM módjától függően változik.

## <a name="next-steps"></a>Következő lépések
További információ az Azure tevékenységi naplóit és erőforrástulajdonság-meghatározások toolearn lásd:

* [Auditálási műveletek a Resource Managerben](../azure-resource-manager/resource-group-audit.md)

További információ a Security Center toolearn hello következő lásd:

* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – további hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – hello legújabb Azure biztonsági hírek és információ lekérése.
