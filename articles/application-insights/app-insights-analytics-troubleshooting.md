---
title: Azure Application Insights az Analytics aaaTroubleshoot |} Microsoft Docs
description: "Problémák az Application Insights analytics? Itt érdemes kezdenie. "
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 9bbd5859-3584-4d80-9b6d-d5910fa48baa
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/11/2016
ms.author: bwren
ms.openlocfilehash: e3be2fbc0237440d3b8a736484434a9f276296c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-analytics-in-application-insights"></a>Elemzés hibaelhárítása az Application Insights szolgáltatásban
Problémák [Application Insights Analytics](app-insights-analytics.md)? Itt érdemes kezdenie. Analytics egy hello hatékony keresési eszköz az Azure Application insights szolgáltatással.

## <a name="limits"></a>Korlátok
* Jelenleg a lekérdezési eredmények korlátozott toojust: hetente az elmúlt adatok keresztül.
* A teszteljük böngészők: Chrome, a peremhálózati és az Internet Explorer legújabb verziója.

## <a name="known-incompatible-browser-extensions"></a>Ismert kompatibilis böngészőbővítmények
* Ghostery

Hello bővítmény letiltása, vagy használjon egy másik böngészőben.

## <a name="e-a"></a>"Nem várt hiba"
![Váratlan hiba képernyő](./media/app-insights-analytics-troubleshooting/010.png)

Belső hiba történt a portál futásidőben – nem kezelt kivétel.

* Hello a gyorsítótár törlése. 

## <a name="e-b"></a>403... Kérjük, próbálkozzon tooreload
![403... Kérjük, próbálkozzon tooreload](./media/app-insights-analytics-troubleshooting/020.png)

Egy hitelesítési kapcsolatos hiba történt (hitelesítés vagy során a hozzáférési jogkivonatok létrehozásához). Előfordulhat, hogy hello portal semmilyen módon nem túl helyreállítása böngésző beállításainak módosítása nélkül.

* Győződjön meg arról [külső cookie-k engedélyezettek](#cookies) hello böngészőben. 

## <a name="authentication"></a>403... Ellenőrizze a biztonsági zóna
![403.. .verify biztonsági zóna](./media/app-insights-analytics-troubleshooting/030.png)

Egy hitelesítési kapcsolatos hiba történt (hitelesítés vagy során a hozzáférési jogkivonatok létrehozásához). Előfordulhat, hogy hello portal semmilyen módon nem túl helyreállítása böngésző beállításainak módosítása nélkül.

1. Győződjön meg arról [külső cookie-k engedélyezettek](#cookies) hello böngészőben. 
2. Használta a kedvenc, könyvjelző vagy mentett hivatkozást tooopen hello Analytics portál? Jelentkezett be, mint amennyi hello hivatkozás mentésekor használt más hitelesítő adatokkal van?
3. Próbálja meg egy-az-private vagy incognito böngésző ablakban (után minden ilyen windows bezárása) használatával. A hitelesítő adatokat kell tooprovide. 
4. Nyisson meg egy másik (normál) böngészőablakot, és nyissa meg túl[Azure](https://portal.azure.com). Jelentkezzen ki. Ezután nyissa meg a hivatkozást, és hello megfelelő hitelesítő adatokkal jelentkezhetnek be.
5. Széle és az Internet Explorer is is felhasználónál Ez a hiba fog történni megbízható helyek zónájába beállítások nem támogatottak.
   
    Győződjön meg arról is [Analytics portál](https://analytics.applicationinsights.io) és [Azure Active Directory portálon](https://portal.azure.com) a hello ugyanazt biztonsági zóna:
   
   * Az Internet Explorerben nyissa meg a **Internetbeállítások**, **biztonsági**, **megbízható helyek**, **helyek**:
     
     ![Internetbeállítások párbeszédpanel, ha egy helyet tooTrusted helyek](./media/app-insights-analytics-troubleshooting/033.png)
     
     Hello webhelyek listája, a következő URL-címek hello érhetők el, ha győződjön meg arról, hogy hello mások szerepelnek is:
     
     https://Analytics.applicationinsights.IO<br/>
     https://login.microsoftonline.com<br/>
     https://login.windows.net

## <a name="e-d"></a>404 ... Nem található az erőforrás
![404... erőforrás nem található](./media/app-insights-analytics-troubleshooting/040.png)

Alkalmazás-erőforrás törölve lett az Application Insights, és már nem érhető el. Ez akkor fordulhat elő, ha mentett hello URL-cím toohello Analytics lap.

## <a name="e-e"></a>403 ... Nincs engedély
![403... nem engedélyezett](./media/app-insights-analytics-troubleshooting/050.png)

Nincs engedélye tooopen az alkalmazás Analytics.

* Kapott hello hivatkozás a valaki más? Kérje meg őket arról, hogy Ön hello toomake [olvasók vagy az erőforráscsoport közreműködők](app-insights-resources-roles-access-control.md).
* Menti hello kapcsolat más hitelesítő adatok használatával? Nyissa meg hello [Azure-portálon](https://portal.azure.com), és jelentkezzen ki, majd próbálja meg a hivatkozás újra, adja meg hello megfelelő hitelesítő adatokat.

## <a name="html-storage"></a>403 ... HTML5-tárolóra
A portál HTML5 localStorage és sessionStorage használja.

* Chrome: Beállítások, adatvédelmi, tartalombeállításait.
* Az Internet Explorer: Internetbeállítások, Speciális lap biztonsági, DOM tárolási engedélyezése

![403... Próbálja tooenable HTML5-tároló](./media/app-insights-analytics-troubleshooting/060.png)

## <a name="e-g"></a>404 ... Nem található előfizetés
![404 ... Nem található előfizetés](./media/app-insights-analytics-troubleshooting/070.png)

hello URL-cím érvénytelen. 

* Nyissa meg az alkalmazás-erőforrást hello [Application Insights portál](https://portal.azure.com). Ezután használja az hello Analytics gombra.

## <a name="e-h"></a>404... a lap nem található
![404 ... A lap nem található.](./media/app-insights-analytics-troubleshooting/080.png)

hello URL-cím érvénytelen.

* Nyissa meg az alkalmazás-erőforrást hello [Application Insights portál](https://portal.azure.com). Ezután használja az hello Analytics gombra.

## <a name="cookies"></a>Cookie-k engedélyezése
  Lásd: [hogyan toodisable harmadik fél cookie-k](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), de figyelje meg, igazolnia kell túl**engedélyezése** őket.


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

