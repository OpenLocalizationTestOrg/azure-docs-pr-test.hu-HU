---
title: "a DirSync és Azure AD Sync aaaUpgrade |} Microsoft Docs"
description: "Ismerteti, hogyan tooupgrade a DirSync és Azure AD Sync tooAzure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: bd68fb88-110b-4d76-978a-233e15590803
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d465b137fb54f0c6e28ccf73429c4bd3aafd8698
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-windows-azure-active-directory-sync-and-azure-active-directory-sync"></a>Frissítse a Windows Azure Active Directory-szinkronizálás és az Azure Active Directory-szinkronizálás
Az Azure AD Connect van hello a legjobb módja tooconnect a helyszíni címtár és az Azure AD és az Office 365. Ez az egy ideje tooupgrade tooAzure AD-csatlakozás a Windows Azure Active Directory-szinkronizálás (DirSync) vagy az Azure AD Sync mivel ezek az eszközök most elavult és támogatásuk 2017. április 13 megszűnik.

hello két identitás szinkronizálási eszközt, amely elavult kínált egyerdős vevők (DirSync) és a több erdőt és egyéb speciális fogyasztóknak (Azure AD Sync). Ezek az eszközök régebbi helyett, amelyet a összes forgatókönyv egyetlen megoldás: az Azure AD Connect. Új funkciókat, a továbbfejlesztett szolgáltatásokat és az új forgatókönyvek támogatása kínál. toobe képes toocontinue toosynchronize a helyszíni identitás adatok tooAzure AD és az Office 365, Határozottan javasoljuk, hogy frissítse a tooAzure AD Connect.

2014. július DirSync utolsó kiadása hello jelent meg, és hello utolsó kiadásban az Azure AD Sync 2015. május jelent meg.

## <a name="what-is-azure-ad-connect"></a>Mi az az Azure AD Connect?
Az Azure AD Connect hello követő tooDirSync és az Azure AD Sync. Ez a két támogatott egyesít minden forgatókönyvben. További információk a [a helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).

## <a name="deprecation-schedule"></a>Érvénytelenítése ütemezése
| Dátum | Megjegyzés |
| --- | --- |
| 2016. április 13. |Windows Azure Active Directory Sync ("DirSync") és a Microsoft Azure Active Directory Sync ("Azure AD Sync") jelentette be, történő használata elavult. |
| 2017. április 13. |Támogatja a végződik. Az ügyfelek nem lesznek képesek tooopen tooAzure AD frissítés nélkül támogatási esetet először csatlakozzon. |
|2017. december 31.|Az Azure AD nem fogadja el a Windows Azure Active Directory Sync ("DirSync") és a Microsoft Azure Active Directory Sync ("Azure AD Sync") érkező kommunikációt.

## <a name="how-tootransition-tooazure-ad-connect"></a>Hogyan tootransition tooAzure AD Connect
Ha futtatja a DirSync, két módon frissítheti: helybeni frissítést, és párhuzamos központi telepítés. A legtöbb felhasználó helyben frissítés ajánlott, és ha van egy nemrég végrehajtott operációs rendszer és a kisebb, mint 50 000 objektummal. Más esetekben ajánlott a DirSync-konfiguráció esetén párhuzamos üzembe helyezést toodo áthelyezése tooa az Azure AD Connect futtató új kiszolgálóra.

Azure AD Sync használja, ha egy frissítés ajánlott. Ha szeretné, akkor lehetséges tooinstall egy új Azure AD Connect-kiszolgáló párhuzamosan, és hajtsa végre az Azure AD Sync-kiszolgáló tooAzure AD egy mozgó áttelepítés Connect.

| Megoldás | Forgatókönyv |
| --- | --- |
| [Frissítés a DirSync szolgáltatásról](active-directory-aadconnect-dirsync-upgrade-get-started.md) |<li>Ha egy meglévő DirSync-kiszolgálóval.</li> |
| [Frissítés Azure AD-szinkronizálóról](active-directory-aadconnect-upgrade-previous-version.md) |<li>Ha az Azure AD-Szinkronizálóról.</li> |

Ha azt szeretné, toosee hogyan toodo helyben frissíthet a DirSync tooAzure AD Connect, majd tekintse meg ezt a Channel 9 videót:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-in-place-upgrade-from-legacy-tools/player]
>
>

## <a name="faq"></a>GYIK
**K: kaptam e-mailben értesítést hello Azure csapata és/vagy az Office 365 üzenetközpont hello üzenetét, de használom a csatlakozás.**  
az Azure AD Connect használatával 1.0 buildszámot toocustomers is küldött hello értesítést. \*.0 (előtti-1.1 kiadási használatával). A Microsoft azt javasolja, hogy az ügyfelek aktuális az Azure AD Connect toostay feloldja a. Hello [automatikus frissítés](active-directory-aadconnect-feature-automatic-upgrade.md) 1.1 bevezetett szolgáltatás révén könnyen tooalways telepítve az Azure AD Connect legújabb verziójával rendelkezik.

**K: fog DirSync vagy az Azure AD Sync működni azon 2017. április 13?**  
A DirSync vagy az Azure AD Sync toowork továbbra is a április 13 2017.  Azonban az Azure AD nem fogadja el a DirSync vagy az Azure AD Sync folytatott kommunikáció a December 31-edik 2017.

**K: DirSync verzióinak frissíthető?**  
Támogatott tooupgrade bármely DirSync kiadásáról jelenleg használatban.

**K: Mi a helyzet hello Azure AD-összekötő FIM vagy MIM?**  
Azure AD-összekötő FIM vagy MIM hello rendelkezik **nem** lett bejelentette történő használata elavult. Ez ugyanis azonos **szolgáltatás rögzíteni**; új funkció sem kerül, és nem hibajavítások kap. A Microsoft azt javasolja, tooplan toomove használja az ügyfelek tooAzure AD Connect. Kifejezetten ajánlott toonot start minden új üzemelő használja azt. Ez az összekötő bejelentések jövőbeli hello már elavult.

## <a name="additional-resources"></a>További források
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
