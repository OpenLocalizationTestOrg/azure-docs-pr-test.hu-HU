---
title: "Active Directory Reporting késések aaaAzure |} Microsoft Docs"
description: "Az események tooshow végzi a jelentéseket az Azure Active Directoryban szükséges idő"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 346b14f8-d16d-4b07-8211-e6c5eec07062
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 14367d21dfb28359f991037cc924d416420be456
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-report-latencies"></a>Az Azure Active Directory-jelentés késleltetései
*Ebben a dokumentációban hello része [Azure Active Directory-jelentéskészítés – útmutató](active-directory-reporting-guide.md).*

| Jelentés | Minimális | Átlagos | Maximális |
| --- | --- | --- | --- |
| **Biztonsági jelentések** | | | |
| Rendszertelen bejelentkezési tevékenység |2 óra |4 óra |8 óra |
| Bejelentkezések ismeretlen forrásokról |2 óra |4 óra |8 óra |
| Több hibát követő bejelentkezések |2 óra |4 óra |8 óra |
| Bejelentkezések különböző földrajzi régiókból |2 óra |4 óra |8 óra |
| Bejelentkezések gyanús tevékenységeket mutató IP-címekkel |2 óra |4 óra |8 óra |
| Bejelentkezések potenciálisan fertőzött eszközökről |2 óra |4 óra |8 óra |
| Rendellenes bejelentkezési tevékenységet mutató felhasználók |2 óra |4 óra |8 óra |
| Felhasználók, akiknek kiszivárogtak a hitelesítő adatai |2 óra |4 óra |8 óra |
| Minden felhasználói bejelentkezési tevékenység |2 óra |4 óra |8 óra |
| **Alkalmazás-jelentések** | | | |
| Tevékenység-kiépítési |2 óra |4 óra |8 óra |
| Alkalmazás-kiépítési hibák |2 óra |4 óra |8 óra |
| Az alkalmazás használatának |2 óra |4 óra |8 óra |
| Jelszó-helyettesítő állapota |2 óra |4 óra |8 óra |
| **Naplózási és tevékenységjelentéseket** | | | |
| Ellenőrzési jelentés |1 perc |15 perc |30 perc |
| Jelszó-visszaállítási tevékenység (az Azure AD) |2 óra |4 óra |8 óra |
| Jelszó-visszaállítási tevékenység (Identity Manager) |2 óra |4 óra |8 óra |
| Jelszó-visszaállítási regisztrációs tevékenység (az Azure AD) |2 óra |4 óra |8 óra |
| Jelszó-visszaállítási regisztrációs tevékenység (Identity Manager) |2 óra |4 óra |8 óra |
| Önkiszolgáló csoportok tevékenységéről (az Azure AD) |2 óra |4 óra |8 óra |
| Önkiszolgáló csoportok tevékenységéről (Identity Manager) |2 óra |4 óra |8 óra |
| **RMS-jelentések** | | | |
| Legtöbb aktív RMS-felhasználók |2 óra |4 óra |8 óra |
| RMS-használat |2 óra |4 óra |8 óra |
| RMS-eszköz használata |2 óra |4 óra |8 óra |
| RMS-kompatibilis alkalmazások használatát |2 óra |4 óra |8 óra |

