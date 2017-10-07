---
title: "interaktív Azure Application Insights-munkafüzetek aaaInvestigate és megosztási használati adatok |} Microsoft docs"
description: "Webes alkalmazása felhasználóit demográfiai elemzése."
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 06/12/2017
ms.author: bwren
ms.openlocfilehash: bdcebe0f97fdad0a0b301df5950dc09698f5a4dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="investigate-and-share-usage-data-with-interactive-workbooks-in-application-insights"></a>Vizsgálja meg és használati adatok megosztása az Application Insightsban interaktív munkafüzetek

Munkafüzetek egyesítése [Azure Application Insights](app-insights-overview.md) adatmegjelenítésekkel, [elemzési lekérdezések](app-insights-analytics.md), és interaktív dokumentumok szövegdobozba. Munkafüzetekhez a többi csoport tagjai az access toohello azonos Azure-erőforrás. Ez azt jelenti, hello lekérdezések és -vezérlők használt toocreate munkafüzet elérhető tooother személy éri el az hello munkafüzet, így azok könnyen tooexplore, kiterjesztése, és ellenőrizze a hibákat.

Munkafüzetek hasznosak a helyzetekben, például:

* Hello használata az alkalmazás fel, ha fontos hello metrikák előzetesen nem tudja: számok felhasználók, adatmegőrzési arányt, átváltási, stb. Egyéb használati elemzőeszközök az Application Insightsban, eltérően munkafüzetek lehetővé teszik, hogy több típusú képi megjelenítések és elemzéseket, szabad formátumú feltárása az ilyen nagyszerű minősítené kombinálni.
* Hogyan működik-e egy újonnan kiadott szolgáltatás tooyour team foglalja össze, megjelenítő felhasználó álló kulcs kapcsolati és más metrikákkal.
* Megosztás hello eredményei egy A / B kísérletezhet az alkalmazás más a csoport tagjai. Hello hello célokat szöveg kísérletezhet, majd minden egyes használati metrika megjelenítése és Analytics query használják tooevaluate hello kísérletet, és törölje a jelet hívás elemek számára, hogy volt-e mindegyik metrikát fölött vagy alatt-célzott ismertetik.
* Nem tervezett kimaradás hello hatását Reporting hello használata az alkalmazás adatokat, a szöveg magyarázat és az információt a következő lépéseket tooprevent kimaradások számát a jövőbeli hello.

> [!NOTE]
> Az Application Insights-erőforrás Lapmegtekintések vagy egyéni események toouse munkafüzetek kell tartalmaznia. [Ismerje meg, hogyan az alkalmazások toocollect oldalára tooset automatikusan az Application Insights JavaScript SDK hello megtekinti](app-insights-javascript.md).
> 
> 

## <a name="editing-rearranging-cloning-and-deleting-workbook-sections"></a>Szerkesztés, átrendezése, a Klónozás és a munkafüzet szakaszok törlése

A munkafüzet egy készült szakaszok: függetlenül szerkeszthető használati képi megjelenítéseket, diagramok, táblák, text vagy Analytics lekérdezési eredmények.

a munkafüzet szakasz tartalmának megjelenítése tooedit hello kattintson hello **szerkesztése** gombra és toohello sarkában hello munkafüzet szakasz.

![Application Insights munkafüzetek szakasz szerkesztési vezérlők](./media/app-insights-usage-workbooks/editing-controls.png)

1. Ha elkészült szakasz módosításához kattintson a **végzett szerkesztése** hello bal alsó sarkában hello szakaszban található.

2. a szakasz duplikált toocreate kattintson hello **klónozni ebben a szakaszban** ikon. Ismétlődő szakaszok létrehozása egy nagyszerű tooway tooiterate lekérdezés előző ismétlési elvesztése nélkül.

3. egy munkafüzet egy másolat toomove kattintson hello **feljebb** vagy **lejjebb** ikon.

4. a szakasz tooremove véglegesen, kattintson a hello **eltávolítása** ikonra.

## <a name="adding-usage-data-visualization-sections"></a>Használati adatok képi megjelenítés szakaszok hozzáadása

Munkafüzetek beépített használati analytics képi megjelenítések négy típusú kínálnak. Minden ad választ az alkalmazás hello használati közös kérdése. tooadd táblázatokat és diagramokat eltérő a következő négy részeket is vegye fel az elemzés lekérdezés szakaszok (lásd alább).

a felhasználók tooadd, munkamenetek, események vagy megőrzési szakasz tooyour munkafüzet, használjon hello **felhasználó hozzáadása** vagy más megfelelő gomb hello munkafüzet hello alján, vagy bármely szakasz hello alján.

![Felhasználók szakaszban munkafüzetekben](./media/app-insights-usage-workbooks/users-section.png)

**Felhasználók** szakaszok választ "hány felhasználó néhány tekint, vagy használja a webhely néhány funkciója?"

**Munkamenetek** szakaszok választ "hány munkamenetek volt igénybe néhány lap megtekintése vagy a webhely néhány funkció használatával?"

**Események** szakaszok választ "hány alkalommal nem felhasználók néhány a lapnak a megtekintésére vagy használja a webhely néhány szolgáltatást?"

Minden, a következő három szakasz típusú vezérlők és a képi megjelenítések azonos készleteinek kínál hello:

* [További tudnivalók a felhasználói munkamenetek és események szakaszok szerkesztése](app-insights-usage-segmentation.md)
* Váltás hello fő diagram, hisztogram rácsok, automatikus insights és minta felhasználók képi megjelenítések hello segítségével **diagram megjelenítése**, **rácsvonalak megjelenítése**, **megjelenítése Insights**, és **Ezek próbafelhasználók** jelölőnégyzeteket minden szakasz hello tetején.

![A munkafüzet megőrzési szakasz](./media/app-insights-usage-workbooks/retention-section.png)

**Megőrzési** szakaszok választ "Személyek néhány tekint, vagy a használt egyes szolgáltatások egy nap vagy hét, hány visszatért az ezt követő nap vagy hét?"

* [Tudjon meg többet az adatmegőrzési szakaszok szerkesztése](app-insights-usage-retention.md)
* Váltás hello választható általános megőrzési diagram hello segítségével **megjelenítése a teljes megőrzési diagram** hello szakasz hello tetején jelölőnégyzetet.

## <a name="adding-application-insights-analytics-sections"></a>Application Insights Analytics szakaszok hozzáadása

![A munkafüzeteket Analytics szakasz](./media/app-insights-usage-workbooks/analytics-section.png)

az Application Insights Analytics lekérdezési szakasz tooyour munkafüzet tooadd hello használata **hozzáadása Analytics lekérdezési** gomb hello munkafüzet hello alján, vagy bármely szakasz hello alján.

Vegyen fel tetszőleges lekérdezések keresztül az Application Insights adatainak munkafüzetek Analytics lekérdezés szakaszok segítségével. Ez azt jelenti, hogy Analytics lekérdezési szakaszok kell lennie a Ugrás-toofor bármely hello felhasználók, a munkamenetek, az események és a megőrzési, például a fent felsorolt négy eltérő a hellyel kapcsolatos kérdések megválaszolásával:

* Hány kivételek fejeződött a hely throw során hello azonos időszak, a használati csökkenése?
* Mi volt a hello terjesztése lapbetöltési idők néhány lap megtekintésének felhasználók számára?
* Hány felhasználó láthatók a webhely néhány azon lapok készlete, de ez nem valamilyen egyéb lapok? Ez akkor lehet hasznos toounderstand, ha a felhasználók, akik használni a webhely funkciók különböző részhalmazai fürttel rendelkezik (hello használata `join` hello operátor `kind=leftanti` módosító a hello Log Analytics lekérdezési nyelv).

Használjon hello [Naplóelemzési lekérdezése nyelvi referencia](https://docs.loganalytics.io/) kapcsolatos lekérdezések írásáról további toolearn.

## <a name="adding-text-and-markdown-sections"></a>Szöveg- és Markdown-szakaszok hozzáadása

A fejlécére kattintva rendezhető, magyarázatot és magyarázatokkal tooyour munkafüzetek hozzáadása segíti a táblázatokat és diagramokat olyan készlete kapcsolja be a leírásokat. Szöveg részeiben munkafüzetek támogatja hello [Markdown-szintaxis](https://daringfireball.net/projects/markdown/) formázás, például a fejlécére kattintva rendezhető, félkövér, dőlt és felsorolásokat szöveg.

tooadd szöveg szakasz tooyour munkafüzet, használja a hello **szöveget** gomb hello munkafüzet hello alján, vagy bármely szakasz hello alján.

## <a name="saving-and-sharing-workbooks-with-your-team"></a>És munkafüzetek megosztása a munkatársaival

Az Application Insights-erőforrást, vagy hello mentett munkafüzetek **jelentések** titkos tooyou szakaszban vagy hello **megosztott jelentések** hozzáféréssel rendelkező elérhető tooeveryone szakaszban toohello Application Insights-erőforrást. tooview hello erőforrás összes hello munkafüzeteket kattintson hello **nyitott** hello művelet található gombra.

a munkafüzetet, hogy e tooshare **jelentések**:

1. Kattintson a **nyitott** hello művelet sávon
2. Kattintson a hello "..." gomb melletti hello munkafüzet tooshare kívánt
3. Kattintson a **tooShared jelentések áthelyezése**.

Kattintson egy munkafüzet egy hivatkozás, vagy e-mailben, tooshare **megosztás** hello művelet sávon. Ne feledje, hogy hello hivatkozás címzettjei toothis erőforrás hello Azure portál tooview hello munkafüzet kell hozzáférni. toomake módosításokat címzettek kell legalább hello erőforrás-közreműködői jogokat.

a hivatkozás tooa munkafüzet tooan Azure irányítópult toopin:

1. Kattintson a **nyitott** hello művelet sávon
2. Kattintson a hello "..." gomb melletti hello munkafüzet toopin kívánt
3. Kattintson a **PIN-kód toodashboard**.

## <a name="next-steps"></a>Következő lépések

## <a name="next-steps"></a>Következő lépések
- tooenable használati észlel, küldésének megkezdése [egyéni események](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) vagy [lapmegtekintés](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Ha már küld egyéni események vagy Lapmegtekintések, így megismerkedhet hello használati eszközök toolearn hogyan felhasználók használhatja a szolgáltatást.
    - [Felhasználók, munkamenetek, események](app-insights-usage-segmentation.md)
    - [Tölcsérek](usage-funnels.md)
    - [Megőrzés](app-insights-usage-retention.md)
    - [Felhasználói folyamatok](app-insights-usage-flows.md)
    - [Felhasználói környezet hozzáadása](app-insights-usage-send-user-context.md)
    
