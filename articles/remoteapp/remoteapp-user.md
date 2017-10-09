---
title: "egy felhasználó tooyour Azure RemoteApp-gyűjtemény aaaAdd |} Microsoft Docs"
description: "Megtudhatja, hogyan tooadd felhasználók tooyour Azure RemoteApp-gyűjtemény"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 6b751fd2-2b11-499f-a2eb-12cb47f3129b
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 0ae88e04c8bfc2ed55dc963945ed7e9ff687b603
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-a-user-tooyour-azure-remoteapp-collection"></a>Hogyan tooadd egy felhasználó tooyour Azure RemoteApp-gyűjtemény
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Mielőtt a felhasználók láthatnak és az alkalmazások az Azure Remoteappban használja, meg kell toogrant tooyour gyűjteménye érhető el őket. Ez része hello egyszerű: A hello **felhasználói hozzáférés** lapra, adja meg a hello hello felhasználóhoz tartozó fiókadatokat, és kattintson a hello pipára.

Milyen fiókot információra van szüksége? Hello a típusú gyűjtemény alapján létrehozott (felhőalapú vagy hibrid), és hogy a Office 365 ProPlus a gyűjteményben, amely függ.

## <a name="supported-user-identities"></a>Támogatott felhasználói identitások
hello különböző gyűjteménytípusok (felhőalapú vagy hibrid) támogatja a hozzáférés tooapplications különböző felhasználói identitásokat.  

RemoteApp hibrid gyűjtemény meg kell tooset fel az Active Directory tartományi infrastruktúra helyi és a címtár-integráció az Azure Active Directory-bérlő (és opcionálisan egyszeri bejelentkezés). Ezen felül kell toocreate egyes Active Directory-objektumok hello helyszíni címtárban.  

A RemoteApp felhőalapú gyűjtemény minden olyan felhasználó, amely rendelkezik az Azure Active Directory identitások támogatásához lehet adni a felhasználói hozzáférés tooRemoteApp tooinclude Microsoft Accounts.  Az alábbi hello táblázatban találja.

Office 365-felhasználók az Azure Active Directory-felhasználók. Ha az Azure Active Directory hibrid rendelkeznek, Directory szinkronizált fiókokkal, azokat is hozzáférést felhasználó RemoteApp hibrid telepítés.   

Ez a táblázat azonosító mentésre alkalmas a gyűjteményt, és milyen hello Active Directory követelményei vannak a gyors referenciaként használható.

| Felhasználói fiókok | Felhő | Hibrid |
| --- | --- | --- |
| Microsoft-fiók |Igen |Nem |
| Azure Active Directory (Azure AD) | | |
| Csak az Azure AD felhő |Igen |Nem |
| Jelszó-szinkronizálással ADsync |Igen |Igen |
| A jelszó-szinkronizálás nélkül ADsync |Igen |Nem |
| Az AD FS ADsync |Igen |Igen |
| [3. fél Azure támogatott Identitásszolgáltatók](https://msdn.microsoft.com/library/azure/jj679342.aspx) (például a Ping) |Igen |Igen |
| Multi-Factor Authentication |Igen |Igen |

Tekintse meg [további információt](remoteapp-ad.md) vonatkozó Active Directory konfigurálása a RemoteApp.

> [!NOTE]
> hello Azure Active Directory-felhasználók hello bérlői előfizetéshez tartozó kell lennie. (Megtekintheti és módosíthatja az előfizetését hello **beállítások** hello portálon lapon. Lásd: [módosítás hello Azure Active Directory-bérlő RemoteApp által használt](remoteapp-changetenant.md) további információt.)
> 
> 

## <a name="office-365-proplus-user-account-information"></a>Az Office 365 ProPlus felhasználói fiókok adatait
Ha a gyűjtemény hello Office 365 ProPlus sablon rendszerképet használ *vagy* Ha létrehozott egy egyéni lemezképet, amely használja az Office 365, csak engedélyezett hello Office 365-előfizetés rendelkező tooadd Azure Active Directory-felhasználók alapértelmezett tartomány az előfizetéséhez. Lásd: [Office 365 segítségével az Azure RemoteApp](remoteapp-o365.md) további információt.

