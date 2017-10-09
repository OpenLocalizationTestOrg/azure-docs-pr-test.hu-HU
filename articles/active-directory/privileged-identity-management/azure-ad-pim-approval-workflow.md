---
title: "aaaAzure Privileged Identity Management jóváhagyási munkafolyamatok |} Microsoft Docs"
description: "További tudnivalók a jóváhagyási munkafolyamatok Privileged Identity Management (PIM)"
services: active-directory
documentationcenter: 
author: barclayn
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: barclayn
ms.custom: pim
ms.openlocfilehash: 4afaf5c138798a803eb3d3b7905b9361d65792cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="approvals-preview"></a>A jóváhagyások (előzetes verzió)

## <a name="overview"></a>Áttekintés

Privileged Identity Management-jóváhagyásokkal rendelkező szerepkörök toorequire jóváhagyási aktiválás konfigurálása, és válasszon egy vagy több felhasználót vagy csoportot, meghatalmazott jóváhagyóknak. Tartsa hogyan olvasása toolearn tooconfigure szerepköröket, és válassza ki a jóváhagyóknak.

>[!NOTE]
Ne feledje, a szolgáltatás fejlesztés alatt van, és hibákat tapasztalhat. hello funkció leírásával elnevezési konvenciói változhat, és nem tekinthető végső.


## <a name="key-terminology"></a>Kulcs terminológia

*Jogosult szerepkör felhasználói* – az jogosult szerepkör felhasználó tulajdonképpen a szervezeten belül, amely jogosult az Azure AD tooan szerepkört rendelték a felhasználó (szerepkör aktiválást igényel).

*Meghatalmazott jóváhagyó* – egy delegált jóváhagyó egy vagy több egyéni felhasználók számára, vagy az Azure AD a csoportokat a szerepkörök aktiválásához kérelmek jóváhagyása felelős.

## <a name="scenarios"></a>Forgatókönyvek

hello private Preview verziójára hello a következő forgatókönyveket támogatja:

**A kiemelt szerepkör rendszergazda (PRA), a következőket teheti:**

-   [egyes szerepkörök jóváhagyás engedélyezése](#enable-approval-for-specific-roles)

-   [Adja meg a felhasználók és/vagy csoportok jóváhagyó tooapprove kérelmek](#specify-approver-users-and/or-groups-to-approve-requests)

-   [minden kiemelt szerepkört a kérelem és a jóváhagyási előzményeinek megtekintése](#view-request-and-approval-history-for-all-privileged-roles)

**Egy kijelölt jóváhagyó, mint a következő műveletek végezhetők el:**

-   [függőben lévő jóváhagyások (kérelmek) megtekintése](#view-pending-approvals-requests)

-   [jóváhagyhatja vagy elutasíthatja az (egyetlen és tömeges) szerepkör jogosultságszint-emelési kérések](#approve-or-reject-requests-for-role-elevation-single-and/or-bulk)

-   [Adja meg a jóváhagyási elutasítási indokát](#provide-justification-for-my-approval/rejection) 

**Jogosult szerepkör felhasználóként a következő műveletek végezhetők el:**

-   [a jóváhagyást igénylő szerepkört aktiválási kérelmeinek megadása](#request-activation-of-a-role-that-requires-approval)

-   [a kérelem tooactivate hello állapotának megtekintése](#view-the-status-of-your-request-to-activate)

-   [a feladat befejezése az Azure AD, ha az aktiválás jóváhagyva](#complete-your-task-in-azure-ad-if-activation-was-approved)

### <a name="navigation"></a>Navigációs

Frissítettük hello navigációs toosupport jóváhagyásokat

![](media/azure-ad-pim-approval-workflow/image001.png)

hello alapértelmezett kezdőlapja kényelmes hozzáférési tooinformation PIM és hello új jóváhagyások dokumentációhoz biztosít.

![](media/azure-ad-pim-approval-workflow/image002.png)

Azt is hozzáadott új szakasz PIM, "A naplózási előzmények" minden felhasználó részére. Itt található összes hello információk vonatkozó tooyour identitás. Ez magában foglalja a függőben lévő és befejezett kérések, bármely végrehajtott megoldása hello kérelmekkel kapcsolatos döntések, és a múltbeli szerepkör aktiválások egy tetszés szerinti helyre.

![](media/azure-ad-pim-approval-workflow/image003.png)

### <a name="enable-approval-for-specific-roles"></a>Egyes szerepkörök jóváhagyás engedélyezése

egy adott szerepkör tooenable jóváhagyás Directory szerepkörök először kiválasztása hello bal oldali navigációs sávon.

![](media/azure-ad-pim-approval-workflow/image004.png)

Található, és válassza a beállítások a bal oldali navigációs Directory szerepkörök hello

![](media/azure-ad-pim-approval-workflow/image006.png)

Válassza ki a kiemelt szerepköröket:

![](media/azure-ad-pim-approval-workflow/image009.png)

Válassza ki az "Engedélyezés" hello jóváhagyási szakasz megkövetelése:

![](media/azure-ad-pim-approval-workflow/image011.png)

Engedélyezve van, a hello panelen lesz bontsa ki a következő adatok tooshow hello:

![](media/azure-ad-pim-approval-workflow/image013.png)

>[!NOTE]
Ha nem ad meg semmilyen jóváhagyóknak, a hello PRA(s) hello alapértelmezett approver(s) válik. PRA(s) lenne szükséges tooapprove összes aktiválási kérelmek ehhez a szerepkörhöz.

### <a name="specify-approver-users-andor-groups-tooapprove-requests"></a>Adja meg a felhasználók és/vagy csoportok jóváhagyó tooapprove kérelmek

toodelegate jóváhagyási hello választógomb túl "kiválasztása jóváhagyóknak":

![](media/azure-ad-pim-approval-workflow/image015.png)

Hello válassza jóváhagyóknak panel betöltésekor, előfordulhat, hogy keresse meg olyan felhasználót vagy csoportot a hello keresősáv hello tetejéhez vagy hello előre megadott listából válassza, majd kattintson a "Select" befejezésekor:

![](media/azure-ad-pim-approval-workflow/image017.png)

Megjegyzés: Előfordulhat, hogy választania több felhasználó vagy csoport egyszerre.

A kiválasztott kijelölt jóváhagyóknak hello listájában jelenik meg az alább látható módon:

![](media/azure-ad-pim-approval-workflow/image019.png)

tooremove jóváhagyó, egyszerűen kattintson a hello Eltávolítás gomb következő tootheir neve.

További jóváhagyóknak tooadd, ismétlődő hello folyamat.

## <a name="view-request-and-approval-history-for-all-privileged-roles"></a>Minden kiemelt szerepkört a kérelem és a jóváhagyási előzményeinek megtekintése

minden kiemelt szerepkört, a kérelem és a jóváhagyási előzmények tooview naplózási előzmények hello irányítópult közül választhat:

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
Rendezze művelet által hello adatokat, és keressen a "Aktiválási jóváhagyott"

### <a name="view-pending-approvals-requests"></a>Függőben lévő jóváhagyások (kérelmek) megtekintése

Mint egy delegált jóváhagyó kap értesítő e-mailek jóváhagyásra váró kérelem esetén. tooview ezek a kérelmek hello PIM portálon, az irányítópult (a hello új navigációs) válassza hello "függőben lévő jóváhagyási kérelmek" hello lapjának bal oldali navigációs sáv.

![](media/azure-ad-pim-approval-workflow/image023.png)

Ott függőben lévő jóváhagyási kérelmek listáját láthatja:

![](media/azure-ad-pim-approval-workflow/image024.png)

### <a name="approve-or-reject-requests-for-role-elevation-single-andor-bulk"></a>Jóváhagyhatja vagy elutasíthatja az (egyetlen és tömeges) szerepkör jogosultságszint-emelési kérések

Hello kérések tooapprove kívánja vagy megtagadják kiválasztása, majd kattintson a művelet található, amely megfelel a döntés hello gombra:

![](media/azure-ad-pim-approval-workflow/image025.png)

### <a name="provide-justification-for-my-approvalrejection"></a>Adja meg a jóváhagyási elutasítási indokát

Ezzel nyisson meg egy új panel tooapprove vagy elutasítása egyszerre több kérést. Indoklásának beírásához ad helyet a döntést, és kattintson jóváhagyása (vagy megtagadása) hello alsó vagy hello panel:

![](media/azure-ad-pim-approval-workflow/image029.png)

Ha hello lekérő folyamat befejeződött, a hello állapotjelzőben fogja tartalmazni hozott döntés (ebben a példában hello döntési jóváhagyása):

![](media/azure-ad-pim-approval-workflow/image031.png)

### <a name="request-activation-of-a-role-that-requires-approval"></a>A jóváhagyást igénylő szerepkört aktiválási kérelmeinek megadása

A kért aktiválásának hello régi PIM navigációs, vagy az új navigációs hello kezdeményezhet jóváhagyást igénylő szerepkört, a szerepkör aktiválása marad hello folyamatként hello azonos. Egyszerűen válassza ki a szerepkör a hello szerepkörök aktiválása:

![](media/azure-ad-pim-approval-workflow/image033.png)

Ha egy kiemelt szerepkörhöz többtényezős hitelesítést igényel, kéri, hogy a feladat végrehajtásához először:

![](media/azure-ad-pim-approval-workflow/image035.png)

A befejezést, aktiválás és a adja meg a indoklást (ha szükséges):

![](media/azure-ad-pim-approval-workflow/image037.png)

hello kérelmező megjelenik egy értesítés, hogy a kérelem hello jóváhagyásra van:

![](media/azure-ad-pim-approval-workflow/image039.png)

### <a name="view-hello-status-of-your-request-tooactivate"></a>A kérelem tooactivate hello állapotának megtekintése

A függőben lévő kérelem tooactivate hello állapotának megtekintése az új navigációs sávon kell elérni. Hello bal oldali navigációs sávon jelölje ki hello "Saját kérések" lapon:

![](media/azure-ad-pim-approval-workflow/image041.png)

hello lekérési állapota alapértelmezés szerint használt érték túl "Függőben", de minden toosee visszaváltható vagy az elutasított kérelmek.

### <a name="complete-your-task-in-azure-ad-if-activation-was-approved"></a>A feladat befejezése az Azure AD, ha az aktiválás jóváhagyva

Hello kérelmet jóváhagyja a rendszer, ha hello szerepkör aktív, és folytathatja a munkát az ehhez a szerepkörhöz szükséges munka.

![](media/azure-ad-pim-approval-workflow/image043.png)

## <a name="next-steps"></a>Következő lépések

Visszajelzése értékes toous. Adjon érzi, hogy szabad tooshare megjegyzések vagy visszajelzést szeretne küldeni betartásának vizsgálatára Itt!
