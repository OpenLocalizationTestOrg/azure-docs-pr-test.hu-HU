---
title: "aaaCortana Eszközintelligencia AppSource közzétételi útmutató |} Microsoft Docs"
description: "Egy Microsoft partnert, mint az alábbiakban a Cortana Intelligence megoldás tooAppSource kell toofollow toopublish hello lépéseket."
services: machine-learning
documentationcenter: 
author: AnupamMicrosoft
manager: jhubbard
editor: cgronlun
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: anupams;v-bruham;garye
ms.openlocfilehash: 66f26016b5eb709c567e9829aa787ca926e13d9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cortana-intelligence-appsource-publishing-guide"></a>A Cortana Intelligence AppSource közzétételi útmutató

## <a name="overview"></a>Áttekintés
AppSource üzleti döntéshozók (BDMs) toodiscover hello egyetlen célját, és problémamentesen próbálja az üzleti megoldások/alkalmazások által a partnerek és értékelik ki, a Microsoft. A Watch [Ez a videó](https://youtu.be/hpq_Y9LuIB8) toolearn AppSource működése. 

A Microsoft Partner, mint révén valóban kihasználhatja a közzététel AppSource, ha:
- Egy intelligens megoldás/alkalmazás használatával készített [Cortana Intelligence Suite](https://azure.microsoft.com/en-us/suites/cortana-intelligence-suite/?cdn=disable).
- A megoldás vagy alkalmazás-címek egy adott üzleti probléma.
- Ön a beépített modulok vagy szellemi tulajdonra vonatkozó, az ügyfelek felhasználhat viszonylag gyorsan kiszámítható módon.

Vessen egy pillantást [Cortana Intelligence megoldások](https://appsource.microsoft.com/en-us/marketplace/apps?product=cortana-intelligence&page=1) AppSource, amely már közzé. 

Ez a cikk részletesen ismerteti az keresztül hello lépéseket tooget partner tooAppSource közzétett Cortana Intelligence megoldás

## <a name="getting-started"></a>Bevezetés
1. A hello [Partner Közösség előnyei útmutató](https://www.microsoftpartnerserverandcloud.com/_layouts/download.aspx?SourceUrl=Hosted%20Documents/Partner%20Community%20Benefits%20Guide%20-%20Cloud%20and%20Enterprise.pdf) (PDF), lásd: a lap 9 tooget Advanced Analytics partnerként felsorolt.
1. Teljes hello [küldje el az alkalmazás](https://appsource.microsoft.com/en-us/partners/list-an-app) űrlap.

    Hello kérdés *"az alkalmazás a beépített hello termékek kiválasztása*", ellenőrizze a hello **más** jelölőnégyzetet és a lista "Cortana Intelligence" hello vezérlő szerkesztése.
1. A Cortana Intelligence alkalmazások kaphat előkészítve tooAppSource, mielőtt a Cortana Intelligence partneri megoldás informatikai csoport által beolvasása hitelesített. folyamat elindítása tooget, az alkalmazás kitöltésével megkérjük megosztás részleteit megtekintheti képernyő [Cortana Intelligence megoldás értékelési AppSource közzétételi](https://aka.ms/cisappsrceval). Ez a hely jelszóval védett, és hello helyhez tartozik hogyan tooget hello jelszó utasításokat.

## <a name="solution-evaluation-criteria"></a>Megoldás értékelési feltételek
A feltételek hello alkalmazás igényeinek toomeet hello listája
1. Alkalmazásnak kell tooaddress adott üzleti használható case probléma egy adott funkcionális területen az előre definiált értéknövelő minimális konfigurációkat a repeatable módon implementable rövid időn belül.
1. Megoldás használatakor a következő összetevők hello legalább egyikét:

    - HDInsight
    - Machine Learning
    - Data Lake Analytics
    - Stream Analytics
    - Cognitive Services
    - Robotkeretrendszer
    - Analysis Services
    - Microsoft R Server önállóan.
    - R-szolgáltatásokat az SQL 2016 vagy HDInsight prémium
1. Megoldás a legalább $1000 havi / DPOR/kriptográfiai Szolgáltató használatával kell létrehozásakor.
1. Megoldás hello Azure Active Directory összevont egyszeri bejelentkezés (AAD összevont egyszeri bejelentkezés) használata engedélyezve van a felhasználói hitelesítés és az erőforrás hozzáférés-vezérlést jóváhagyását. Szüksége lesz, hogy-e a megoldás AAD team összevont egyszeri Bejelentkezéses engedélyezve van, mielőtt az alkalmazás lehet előkészítve tooAppSource tooshow toohello kiértékelése.

     toosee jelentés toobe AAD összevont egyszeri Bejelentkezéses engedélyezve van, a tooposition 02:35 [AppSource próbaverziója bízná](https://aka.ms/trialexperienceforwebapps) videó. Ha az alkalmazás nem engedélyezték a AAD összevont egyszeri Bejelentkezést, de itt van néhány dokumentációjában
   1. [Egy kattintással bejelentkezés](https://identity.microsoft.com/Landing?ru=https://identity.microsoft.com/).
   1. [Alkalmazások integrálása az Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-an-application).
     
1. Használja a Power bi-ban a megoldásban: kötelező, de erősen ajánlott érdeklődők nagyobb számú toogenerate bizonyítása.

## <a name="devcenter-account-setup"></a>DevCenter fiók beállítása
Ez az a vállalat toobecome közzétevő regisztrálása a Microsoft hello folyamatán. Ha már létező DevCenter fiókkal érvényes kiadó, ossza meg a DevCenter fiókjához társított hello e-mail azonosítója. Az alkalmazás közzététele számára jóvá van hagyva, miután elérhetővé válik hozzáférési toofull dokumentációja [publisher profil felhő Portal kezelése](https://cloudpartner.azure.com/#documentation/manage-publisher-profile)

Ha még nincs fiókja, az alábbiakban hello fontos lépések toosetup egy DevCenter fiókot.
1. Microsoft-fiók létrehozása [Itt](https://signup.live.com/signup.aspx).
1. Nyissa meg a Microsoft DevCenter toohello [regisztrációs lapjához](http://go.microsoft.com/fwlink/?LinkId=615100) , és kattintson az "előfizetés".
1. A fizetési megadását kéri, használja, hogy azt a megadott tooyou hello kódot. Ha még nem rendelkezik egy kódot, forduljon a [ appsourcecissupport@microsoft.com ](mailto:appsourcecissupport@microsoft.com?subject=Request%20for%20promotion-code%20for%20DevCenter%20account%20setup).
1. Jelölje be hello [ország vagy régió](https://docs.microsoft.com/windows/uwp/publish/account-types-locations-and-fees) , ahol él, illetve ahol az üzleti. **Nem fogja tudni toochange ezt később.**
1. Válassza ki a [developer-fiók típusa](https://docs.microsoft.com/windows/uwp/publish/account-types-locations-and-fees) a AppSource, válassza ki **vállalati**. Ahhoz, hogy a vállalati fiók kell, hogy tooreview ezek [irányelvek](https://docs.microsoft.com/windows/uwp/publish/opening-a-developer-account).
1. Adja meg a hello kapcsolattartási adatokat a fejlesztői fiókba toouse használni szeretne.
További részletes részletes információk az útmutató toosetup DevCenter fiókjához, tekintse meg a hello útmutatást [Itt](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-accounts-creation-registration).

## <a name="provide-info-for-microsoft-sellers"></a>A Microsoft eladók adatainak megadása
Hello kulcs értéknövelő AppSource az azokra a partnerekre vonatkozó egyik toobe képes toocollaborate a Microsoft Sellers a lehetséges ügyfeleket elé partneralkalmazások elhelyezésében.

Felfelé kitölti [partneri megoldás információ a következő Microsoft Sellers](https://aka.ms/aapartnerappinfo) túl küldje el[appsourcecissupport@microsoft.com](mailto:appsourcecissupport@microsoft.com?subject=Request%20publisher%20account%20creation%20for%20%3cPartner%20Name%3e%20and%20whitelist%20owner/contributer%20AAD/MSA%20email%20IDs). Ez akkor szükséges lépést a Cortana Intelligence app tooget jóváhagyott közzététel esetén.

## <a name="build-a-compelling-customer-walkthrough-on-appsource"></a>Egy kötelező felhasználói forgatókönyv AppSource létrehozása
Vessen egy pillantást [Neal Analytics készlet optimalizálási](https://appsource.microsoft.com/en-us/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview&tag=CISHome) AppSource a. Minden alkalmazás bejegyzést AppSource számára cím, összefoglalás (legfeljebb 100 karakter), leírás (maximális 1300 karakter), képek, videók (nem kötelező), PDF-dokumentumokat leszámítva belépési pontot egy próbaverziója. Partnerek ki kell használniuk a toobuild egy lenyűgöző felhasználói élményt.

Partnerek kell gondolja, hogy azok magánjellegét, mint egy záró tooend értékesítési vezénylési AppSource a hello tartalom. Az ügyfelek hello nevét és leírását toounderstand hello értékajánlatához olvasható, és folytassa a képek és videók toounderstand arra készül, hogy milyen hello megoldás keresztül. Esettanulmányok érdekében tekintse meg az ügyfelek hogyan meglévő potenciális vevők értéket kap. 

A kell teszi hello vevő összes érzi, hogy további érdekelt és van tooknow. Gondolja, hogy ezek betűközű alapú fedélzetek bemutató jó műszaki értékesítő hello új ügyfelek akkor végezze el. hello javasolt Leírás formátuma toobreak hello szöveget alárendelt szakaszokra tagolhatja értéknövelő, az a kijelölt alárendelt fejléccel. 

Látogatók általában áttekintése hello "ajánlat-összefoglaló" mező és alárendelt fejlécek tooget gist, mely hello alkalmazás címek keresztül, és miért akkor érdemes csak egy gyors áttekintő hello alkalmazás. Ezért fontos tooget hello felhasználó figyelmét számukra egy OK tooread tooget hello részletekről.

Tekintse meg a partnerek tette.
- [Neal Analytics készlet optimalizálása](https://appsource.microsoft.com/en-us/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview)
- [Plexure kereskedelmi személyre szabása](https://appsource.microsoft.com/en-us/product/web-apps/plexure.c82dc2fc-817b-487e-ae83-1658c1bc8ff2?tab=Overview)
- [AvePoint állampolgár szolgáltatások](https://appsource.microsoft.com/en-us/product/web-apps/avepoint.7738ac97-fd40-4ed3-aaab-327c3e0fe0b3?tab=Overview)

hello végső act az értékesítési tooshow egy bemutató hello app/megoldás jelenít meg, hogyan kerül hello értékajánlatához. Ez pontosan mit jelent a AppSource egy ügyfél próbaverziója. Egy jó bemutató hajt végre a következő hello:
- Újra összefoglalója rövid hello értékajánlatához hello alkalmazás és a kimenő hello személyeknek belül hello ügyfél vállalkozás, akik hello megoldás volna interakciót listája
- Mondja el szövegegység és készletek hello környezet konkrét problémák foglalkozó minta ügyfelekkel kapcsolatos
- Azt ismertetik, hogyan hello helyzet általában kezdeményezzenek és hatással lehet hello vevő (előtt) VS, mi történne hello megoldás használatban van. Ez is megjeleníthetők, használja a Power bi-irányítópultok stb.
- Foglalják össze, hogyan hello megoldás lehetővé teszi bármely adott stb Machine Learning algoritmusokkal fordulhat elő.

a AppSource hozzáadni kívánt hello tartalom kiváló minőségű legyen, és fel elegendő tooenable hello következő stitched:
- Egy partner által létrehozott műszaki értékesítési segítsen kell használnia, az értékesítési vezénylési. Az értékesítési csapat használja azt a helyes bejelentkezési MS értékesítési segítsen lehetőség van a képes toodo hello azonos várható. Ez lehetővé teszi az ügyfél kapcsolódási pontot toobe képes tooconsistently továbbítási hello azonos szövegegység tootheir team kocsikísérők és magasabb ups tooget költségvetési és jóváhagyások előtt a beszerzési üzlet végezhető.
- Hello hely biotermeléssel látogató ügyfél halad át hello tartalom minden önmagában, és érzi, hogy várakozással toorespond hátsó toopartner kommunikációs toomove azokat, amelyek a következő lépéseket.

Amely van partnerek kell gondolja át, hogy miért hello azok magánjellegét, mint egy záró tooend értékesítési vezénylési AppSource a tartalom. Hello szövegegység és a szolgáltatások toobe próbaverziója látható alapján ajánlat típusú léphet.

## <a name="publish-your-app-on-hello-publishing-portal"></a>A közzétételi hello Portal alkalmazás közzététele
Miután az alkalmazás azt kiértékelni hello a fenti lépéseket toohello közzétételi portáljára hozzáférést kap, és látható [hogyan toopublish a Cortana Intelligence Cloud Partner portálra keresztül nyújtanak](https://cloudpartner.azure.com/#documentation/cloud-partner-portal-publish-cortana-intelligence-app) lépések részletes ismertetése.

Ha tájékozódhat hello mezők van szüksége, e-mailben < appsourcecissupport@microsoft.com >.
## <a name="my-app-is-published-on-appsource---now-what"></a>Saját alkalmazás közzétett AppSource - most Mi a teendő?
Először is az alkalmazás első Gratulálunk közzé.
hello szint nem tesz közzé az alkalmazást a AppSource fokozottan kapott értéket ad vissza attól függ, hogyan hassanak a hello célközönség. Lásd: [a Cortana Intelligence alkalmazás AppSource növekedési támadásoknak](http://aka.ms/aagrowthhackguide) olvashat, hogyan maximalizálhatja hello értéket ad vissza.

Ha kérdése van, vagy a javaslatok, megkérjük érheti el a toous < appsourcecissupport@microsoft.com >.

