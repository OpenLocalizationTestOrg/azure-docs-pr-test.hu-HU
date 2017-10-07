---
title: "aaaAzure Mobile Engagement hibaelhárítási útmutatója - elemzés"
description: "Az Azure Mobile Engagement Analytics, a figyelés, a szegmentálási és az irányítópult problémák elhárítása"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 04a7020a-ad74-4491-be69-0bd574890029
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 69c6ff8f5c8540f8ba8b85b9ffec55acc59329fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a>Az elemzés, a figyelés, a szegmentálási és az irányítópult problémák hibaelhárítási útmutató
hello az alábbiakban lehetséges problémák merülhetnek fel a hogyan Azure Mobile Engagement az alkalmazások, eszközök és felhasználók adatokat gyűjt.

## <a name="missingdelayed-information"></a>Hiányzó/késleltetett információk
### <a name="issue"></a>Probléma
* Elemzés, Szegmentálás vagy irányítópulton megjelenő adatokat késleltetett.
* Hiányzik az figyelés információ.
* Hiányzik az elemzés, Szegmentálás vagy irányítópult információ.
* Szerezze meg a szegmentálási korlátozza.

### <a name="causes"></a>Okok
* Hello Analytics API figyelő API-t is használhatja, és a szegmensek API toosee, ha bármely adatok hiányoznak a felhasználói felület hello látható hello API-k használatával.
* Ha hello Azure Mobile Engagement SDK nem megfelelően integrálva van az alkalmazás ezután nem fog hello elemzés, szegmentálását, figyelés vagy irányítópultok képes toosee adatokat.
* Szegmensek nem lehet módosítani a létrehozásuk után, szegmensek csak olyan számítógépekre "Klónozott" (másolt) vagy "megsemmisül" (törölve). Szegmensek csak tartalmazhat 10 feltételeket.
* hello hiányoznak egyes adatok megfigyelése legjobb módja tootest toosetup egy vizsgálati eszköz, távolítsa el és/vagy telepítse újra az hello alkalmazást hello vizsgálati eszköz.
* Információk 24 óránként frissül elemzés, Szegmentálás vagy irányítópultok.
* Új szegmensek található információk csak akkor jönnek létre, akkor is, ha az előző információin alapuló hello szegmens követő 24 órában nem jeleníthető meg.
* A felhasználói felület hello analytics adatok szűrése megjeleníti az összes ilyenek például az alkalmazás hello verziótól függetlenül (pl. "Lefagy" név szerint szűrve jelennek meg az 1-es és 2-es verziójának az alkalmazásban).
* hello Analytics időszakát alapul hello felhasználói eszközbeállítások, hello dátum, a felhasználó, akinek telefonhoz hello dátum helytelenül állította be a sikerült megjelennek hello megfelelő időszakra vonatkozóan.
* Nem a rendszer adatokat naplózza hello gomb használatakor kiszolgálóoldali túl "teszt" leküldéses értesítések, adatokat csak naplózza a valódi leküldéses kampányokra.

## <a name="cant-locate-items-in-ui"></a>Nem található elemek a felhasználói felületen
### <a name="issue"></a>Probléma
* Nem hozható létre az egyes beépített alapuló szegmens, vagy egyéni alkalmazásadatok címke feltételek.
* Nem található egyes beépített, vagy egyéni alkalmazásadatok elemzés, figyelés vagy irányítópultok szempontok címkét.
* Elemzés, figyelés, Szegmentálás vagy irányítópultok hello adatokat nem tudja értelmezni.

### <a name="causes"></a>Okok
* Néhány beépített elemek és alkalmazásadatok címkék csak leküldéses feltételként használható, de nem lehet hozzáadni tooa szegmens vagy elemzés, figyelés vagy irányítópulton látható. 
* A beépített elemek és alkalmazásadatok nem lehet címkéket hozzáadni tooa szegmens, szüksége lesz minden olyan funkciókat, mint egy szegmens alapján kampány tooperform hello feltételeket célzó toosetup listája.
* Hello helyi menük hello elemzés, figyelés, Szegmentálás és irányítópultok fejezeteiben hello Azure Mobile Engagement felhasználói felületén, további segítségért tekintse meg, és hogyan tooinformation.

## <a name="crash-troubleshooting"></a>Összeomlás-hibaelhárítás
### <a name="issue"></a>Probléma
* Alkalmazás összeomlik elemzés, figyelés és az irányítópult jelenik meg.

### <a name="causes"></a>Okok
* tootroubleshoot alkalmazás összeomlik elemzés, figyelés vagy irányítópulton látható győződjön meg arról, hogy toocheck hello kibocsátási megjegyzések az ismert problémák hello SDK korábbi verzióival.
* toofurther hibáinak elhárítása az alkalmazás összeomlik végezzen el egy eseményt a vizsgálati eszköz a telepített alkalmazásokkal rendelkező, és keresse meg az Eszközazonosítót hello Azure Mobile Engagement felhasználói felületén hello "figyelője – események" szakaszában. Ezután hajtsa végre az alkalmazás toocrash okozó hello esemény, és további tudnivalókat a hello hello Azure Mobile Engagement felhasználói felület "Figyelője – összeomlás" szakasza. 

