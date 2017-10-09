---
title: "aaaGroup házirend- és mobileszköz-kezelési beállítások |} Microsoft Docs"
description: "Tájékoztatást ad azokról csoportházirend és a mobileszköz-felügyelet (MDM) beállításait a vállalat által birtokolt eszközök használni. Ezek a házirendek az eszköz teljes alkalmazott toohello felhasználói."
services: active-directory
keywords: "Mik azok a csoport házirend és a vállalati Állapothordozás, a vállalati Állapothordozás, a windows felhőalapú MDM beállításait"
documentationcenter: 
author: tanning
manager: femila
editor: curtand
ms.assetid: 6471a9b3-8dd4-4237-89d1-bfbeca9f8252
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 762419b47014b1fb4d92ac528785e20078afe689
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="group-policy-and-mdm-settings"></a>A csoportházirend és a mobileszköz-kezelési beállítások
Használja a csoportházirend és a mobileszközök felügyeletének (MDM) beállításai csak a vállalat által birtokolt eszközök mivel ezek a házirendek teljes alkalmazott toohello felhasználó-eszköz. Egy mobileszköz-kezelési házirend toodisable beállítások szinkronizálásának személyes alkalmazása, felhasználó által birtokolt eszköz lesz negatívan hello használata az adott eszköz. Ezenfelül más felhasználók fiókjait hello eszközön is befolyásolják hello házirend.

Vállalatok számára, hogy szeretné, hogy a személyes (nem felügyelt) eszközök központi toomanage használhatja az Azure portál tooenable hello, vagy tiltsa le a központi, nem pedig a csoportházirend vagy a mobileszköz-kezelést.
a következő táblák hello leírják hello házirend-beállítások.

## <a name="mdm-settings"></a>MDM-beállítások
hello mobileszköz-kezelési házirend beállításait alkalmazni tooboth Windows 10 és Windows 10 Mobile.  Windows 10 Mobile-támogatás csak Microsoft-fiók segítségével a felhasználó OneDrive-fiókja központi létezik.  Tekintse meg túl "Eszközöket és végpontok" című szakaszban talál információt az Azure AD támogatott eszközök alapú szinkronizálása.

| Név | Leírás |
| --- | --- |
| Microsoft-fiók csatlakozás engedélyezése |Lehetővé teszi, hogy a felhasználók tooauthenticate hello eszközön a Microsoft-fiókkal |
| A beállítások szinkronizálásának engedélyezése |Lehetővé teszi, hogy a felhasználók tooroam Windows-beállítások és alkalmazások adatainak; Ez a házirend letiltása letiltja a szinkronizálása, valamint a mobileszközök biztonsági mentések |

## <a name="group-policy-settings"></a>Csoportházirend-beállítások
hello csoportházirend-beállításokkal rendelkező Active Directory-tartományhoz csatlakoztatott tooan tooWindows 10-eszközökre vonatkoznak. hello tábla is örökölt beállítások toomanage szinkronizálási beállítások, amelyek megjelenik, de, amely nem működnek a vállalati állapot barangolás a Windows 10, amely a "Nem használható" hello leírás leírásban ki vannak emelve.

| Név | Leírás |
| --- | --- |
| Fiókok: Blokk-Microsoft-fiókok |A házirend-beállítás megakadályozza, hogy a felhasználók új Microsoft-fiókokat adjanak hozzá ezen a számítógépen |
| Szinkronizálja a |Megakadályozza, hogy a felhasználók tooroam Windows-beállítások és alkalmazások adatainak |
| Szinkronizálja a személyre szabás |Letiltja a hello témák csoport szinkronizálása |
| Szinkronizálja a böngésző beállításai |Letiltja a hello Internet Explorer csoport szinkronizálása |
| Szinkronizálja a jelszavakat |Letiltja a jelszavak csoport szinkronizálása |
| Szinkronizálja a más Windows-beállítások |Letiltja a Windows egyéb beállítások csoport szinkronizálása |
| Szinkronizálja a asztal személyre szabása |Ne használjon; nincs hatása |
| Szinkronizálja a forgalmi díjas kapcsolatok használata esetén |Letiltja a roaming díjas kapcsolatok, például a 3 G mobilhálózat |
| Szinkronizálja a alkalmazások |Ne használjon; nincs hatása |
| Szinkronizálja a Alkalmazásbeállítások |Alkalmazásadatok központi letiltása |
| Nem kezdő beállítások szinkronizálása |Ne használjon; nincs hatása |

## <a name="related-topics"></a>Kapcsolódó témakörök
* [A vállalati Állapothordozás áttekintése](active-directory-windows-enterprise-state-roaming-overview.md)
* [Az Azure Active Directoryban a vállalati állapothordozás engedélyezése](active-directory-windows-enterprise-state-roaming-enable.md)
* [Beállítások és adatok hordozása – gyakori kérdések](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Windows 10 központi beállításainak ismertetése](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [hibaelhárítással](active-directory-windows-enterprise-state-roaming-troubleshooting.md)

