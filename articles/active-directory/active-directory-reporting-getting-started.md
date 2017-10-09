---
title: "Azure Active Directory Reporting: első lépések | Microsoft Docs"
description: "Listák hello az Azure Active Directory reportingban elérhető különböző jelentéseket"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 7ac99919-8df5-4424-9298-fc7c025ba949
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: f47875708398391dd7f3efdc56a741fdba273b76
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-active-directory-reporting"></a>Bevezetés az Azure Active Directory Premium Reporting használatába
## <a name="what-it-is"></a>Mi ez?
Az Azure Active Directory (Azure AD) biztonsági, naplózási és tevékenységjelentéseket biztosít a címtárához. Itt olvashat egy listát hello jelentéseket tartalmazza:

### <a name="security-reports"></a>Biztonsági jelentések
* Bejelentkezések ismeretlen forrásokról
* Több hibát követő bejelentkezések
* Bejelentkezések különböző földrajzi régiókból
* Bejelentkezések gyanús tevékenységeket mutató IP-címekkel
* Rendszertelen bejelentkezési tevékenység
* Bejelentkezések potenciálisan fertőzött eszközökről
* Rendellenes bejelentkezési tevékenységet mutató felhasználók

### <a name="activity-reports"></a>Tevékenységjelentések
* Alkalmazáshasználat: összegzés
* Alkalmazáshasználat: részletes
* Alkalmazás irányítópultja
* Alkalmazás-kiépítési hibák
* Egyéni felhasználói eszközök
* Egyéni felhasználói tevékenység
* Csoporttevékenység-jelentés
* Jelszó-visszaállítási regisztrációs tevékenységjelentés
* Jelszó-visszaállítási tevékenység

### <a name="audit-reports"></a>Naplózási jelentések
* Címtárnaplózási jelentés

> [!TIP]
> Az Azure AD Reporting további dokumentációiért lásd: [View your access and usage reports](active-directory-view-access-usage-reports.md) (A hozzáférési és használati jelentések megtekintése).
> 
> 

## <a name="how-it-works"></a>Működés
### <a name="reporting-pipeline"></a>Jelentéskészítési folyamat
hello jelentéskészítési folyamat három fő lépésből áll. Minden alkalommal, amikor egy felhasználó bejelentkezik, vagy hitelesítéskor, hello következő történik:

* Először hello felhasználó hitelesítése (sikeresen vagy sikertelenül), és hello eredmény hello Azure Active Directory szolgáltatás adatbázisaiban tárolja.
* Rendszeres időközönként minden friss bejelentkezést feldolgoz. Ezen a ponton a rendellenes tevékenységeket észlelő, illetve biztonságos algoritmusok gyanús tevékenységeket keresnek az összes legutóbbi bejelentkezésben.
* A feldolgozás után hello jelentések írása, a gyorsítótárba, és kiad a klasszikus Azure portálon hello.

### <a name="report-generation-times"></a>Előállítási idők jelentése
Lejáró toohello nagy mennyiségű hitelesítések és bejelentkezési hello által feldolgozott modulok az Azure AD platform hello legutóbbi bejelentkezések feldolgozása, átlagosan egy órával korábbiak. Ritka esetekben too8 óra tooprocess hello legutóbbi bejelentkezések azt is tarthat.

Hello súgószöveg el az egyes jelentések tetején hello megvizsgálásával hello legutóbbi feldolgozott bejelentkezés találja.

![Az egyes jelentések hello tetején súgószöveg](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [!TIP]
> Az Azure AD Reporting további dokumentációiért lásd: [View your access and usage reports](active-directory-view-access-usage-reports.md) (A hozzáférési és használati jelentések megtekintése).
> 
> 

## <a name="getting-started"></a>Bevezetés
### <a name="sign-into-hello-azure-classic-portal"></a>Jelentkezzen be hello a klasszikus Azure portálon
Először, szüksége lesz toosign történő hello [a klasszikus Azure portálon](https://manage.windowsazure.com) globális vagy megfelelőségi rendszergazdaként. Egy Azure-előfizetés szolgáltatás-rendszergazda vagy társadminisztrátorának kell lennie, vagy használja hello "hozzáférési tooAzure AD" Azure-előfizetés.

### <a name="navigate-tooreports"></a>Keresse meg a tooReports
tooview jelentések, keresse meg a toohello jelentések lapot a címtár hello tetején.

Ha az első alkalommal hello jelentések megtekintése, szüksége lesz tooagree tooa párbeszédpanel hello jelentések megtekintése előtt. Ez az, hogy a rendszer a szervezet tooview rendszergazdái elfogadható tooensure ezeket az adatokat, amelyek lehet, hogy egyes országokban bizalmas információnak számítanak.

![Párbeszédpanel](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a>Az egyes jelentések megismerése
Keresse meg minden jelentés toosee hello gyűjtött adatok típusától és hello bejelentkezések feldolgozása. Megtalálhatja a [összes hello jelentések itt listája](active-directory-reporting-guide.md).

![Minden jelentés](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-hello-reports-as-csv"></a>Hello jelentések letöltése CSV-ként
Az egyes jelentések letölthetők CSV-fájlként (vesszővel tagolt adatfájlként). Ezeket a fájlokat, az Excel, a powerbi-ban vagy a programok toofurther elemezheti az adatokat külső analysis is használhatja.

a jelentés a fürt megosztott kötetei toodownload keresse meg a toohello jelentés, és kattintson a "Letöltés" hello lap alján.

![Letöltés gomb](./media/active-directory-reporting-getting-started/downloadButton.png)

> [!TIP]
> Az Azure AD Reporting további dokumentációiért lásd: [View your access and usage reports](active-directory-view-access-usage-reports.md) (A hozzáférési és használati jelentések megtekintése).
> 
> 

## <a name="next-steps"></a>Következő lépések
### <a name="customize-alerts-for-anomalous-sign-in-activity"></a>Rendellenes bejelentkezési tevékenységek riasztásainak testreszabása
Keresse meg a könyvtár toohello "Beállítása" lapon.

Görgessen toohello "Értesítések" szakaszhoz.

Engedélyezi vagy letiltja a hello "E-mail-értesítések a rendellenes bejelentkezések" szakaszhoz.

![hello értesítések szakasz](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-hello-azure-ad-reporting-api"></a>Azure AD jelentéskészítési API hello integrálása
Lásd: [Ismerkedés a Reporting API hello](active-directory-reporting-api-getting-started.md).

### <a name="engage-multi-factor-authentication-on-users"></a>Multi-Factor Authentication engedélyezése a felhasználók számára
Válasszon ki egy felhasználót egy jelentésben.

Gombra hello "MFA engedélyezése" hello hello képernyő aljára.

![hello multi-factor Authentication gomb üdvözlő képernyőt hello aljához](./media/active-directory-reporting-getting-started/mfaButton.png)

> [!TIP]
> Az Azure AD Reporting további dokumentációiért lásd: [View your access and usage reports](active-directory-view-access-usage-reports.md) (A hozzáférési és használati jelentések megtekintése).
> 
> 

## <a name="learn-more"></a>Részletek
### <a name="audit-events"></a>Események naplózása
Ismerje meg, milyen eseményeket naplóz hello címtár [Azure Active Directory Reporting Audit Events](active-directory-reporting-audit-events.md).

### <a name="api-integration"></a>API-integráció
Lásd: [Ismerkedés a Reporting API hello](active-directory-reporting-api-getting-started.md) és hello [API-referenciadokumentáció](https://msdn.microsoft.com/library/azure/mt126081.aspx).

### <a name="get-in-touch"></a>Kapcsolatfelvétel
Az [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) e-mail címre bármilyen észrevételét, problémáját vagy kérdését elküldheti.

> [!TIP]
> Az Azure AD Reporting további dokumentációiért lásd: [View your access and usage reports](active-directory-view-access-usage-reports.md) (A hozzáférési és használati jelentések megtekintése).
> 
> 

