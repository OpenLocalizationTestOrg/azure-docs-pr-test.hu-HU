---
title: "aaaAzure Team adatok tudományos folyamat áttekintése |} Microsoft Docs"
description: "Adatok tudományos módszertana toodeliver prediktív elemzési megoldásokat és intelligens alkalmazások."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b1f677bb-eef5-4acb-9b3b-8a5819fb0e78
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: bradsev;
ms.openlocfilehash: 2ba03585a6f6f855faaa3b5c0c75149cad0a88bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="team-data-science-process-overview"></a>Team adatok tudományos folyamat áttekintése

hello Team adatok tudományos folyamat (TDSP) egy gyors iteratív adatok tudományos módszertana toodeliver prediktív elemzési megoldások és intelligens alkalmazások hatékony. TDSP javítja a csoportos együttműködés és megismerését. Tartalmaz egy lepárlása hello ajánlott eljárások és szerkezetek a Microsoft és más hello iparágban hello adatok tudományos kezdeményezések sikeres végrehajtásának megkönnyítése érdekében. hello célja toohelp vállalatok teljes mértékben vegye figyelembe az analytics program hello előnyeit.

Ez a cikk áttekintése TDSP és fő összetevőit. Számos különböző eszközök nyújtunk általános leírását itt hello folyamat, amely végrehajtható. Részletesebb leírását hello tevékenységeket és hello életciklusának hello folyamat az érintett szerepkörök további csatolt témakörökben találhatók. Hogyan is biztosít a Microsoft eszközök és infrastruktúra tooimplement hello TDSP használjuk a csoportban az adott használatával TDSP tooimplement hello útmutatást.

## <a name="key-components-of-hello-tdsp"></a>Hello TDSP fő összetevői

A legfontosabb összetevők a következő hello TDSP foglalja magában:

- A **adatok tudományos életciklus** meghatározása
- A **szerkezetének szabványosított**
- **Infrastruktúra- és erőforrások** tudományos projektekhez
- **Eszközök és segédprogramok** projekt végrehajtásra


## <a name="data-science-lifecycle"></a>Adatok tudományos életciklusa

hello Team adatok tudományos folyamat (TDSP) biztosítja az adatok tudományos projektek életciklus toostructure hello fejlesztői. hello életciklus hello lépéseit, a start toofinish, amelyek projektek általában végrehajtás sorolja fel.

Ha az adatok egy másik tudományos életciklusát, például a [SZÍNELOSZLÁS-DM](https://wikipedia.org/wiki/Cross_Industry_Standard_Process_for_Data_Mining), [KDD](https://wikipedia.org/wiki/Data_mining#Process) vagy a szervezet saját egyéni folyamat továbbra is használhatja ezeket a fejlesztői környezetében hello feladatalapú TDSP hello életciklusának. Magas szinten a különböző módszereit, amelyek rendelkeznek nagy közös. 

Életciklus úgy tervezték, adatok tudományos projektek intelligens alkalmazások részét képezi. Ezek az alkalmazások központi telepítése a machine learning vagy mesterséges eszközintelligencia modellek prediktív elemzési. Felderítési adatok tudományos projektek vagy alkalmi analytics projektek is is előnyt az ezt a folyamatot. Azonban ebben az esetben egyes leírt hello lépések nincs feltétlenül szükség.    

hello TDSP életciklus ismételt végrehajtott öt fő szakaszból áll:

* **Üzleti ismertetése**
* **Adatok megszerzését és ismertetése**
* **Modellezési**
* **Üzembe helyezés**
* **Ügyfelek**

Íme hello vizuális ábrázolását **Team adatok tudományos folyamat életciklus**. 

![TDSP-életciklus](./media/data-science-process-overview/tdsp-lifecycle.png) 

hello célokat, feladatok és minden szakaszra hello életciklus TDSP a dokumentáció összetevők ismertetett hello [Team adatok tudományos folyamat életciklus](data-science-process-lifecycle.md) témakör. Ezekről a feladatokról és az összetevők társított projekt szerepkörök:

- Megoldásokért felelős mérnök
- Projektmenedzsert
- Adatelemző
- Projektvezető 

hello következő ábra áttekintést nyújt a rács (kékkel jelölt) hello feladatok és összetevők (zöld) a hello életciklus (a hello vízszintes tengely) minden szakaszhoz tartozó ezeket a szerepköröket (a függőleges tengely hello). 

![TDSP-szerepkörök-és-feladatok](./media/data-science-process-overview/tdsp-tasks-by-roles.png)

## <a name="standardized-project-structure"></a>Szabványos projekt struktúra

Minden projekt könyvtárstruktúra megosztása és sablonok használata a projekt dokumentumok rendelkező megkönnyíti a hello csapat tagjai toofind a projektek adatait. A verziókezelő rendszert (virtuális) például a Git, TFS vagy Subversion tooenable Csoportalapú együttműködés kód és a dokumentumok tárolódnak. Feladatok és a szolgáltatások nyomkövetési rendszer például Jira gyors projektben nyomon követése, Rally, a Visual Studio Team Services lehetővé teszi, hogy az egyedi szolgáltatások hello kód közelebb nyomon követése. Ilyen követési azt is lehetővé teszi, hogy a csoportok tooobtain jobban a becsült költség. TDSP azt javasolja, hogy egy külön tárház minden olyan projekthez hello virtuális versioning, információ-biztonság és együttműködés létrehozását. hello összes projektek segít build intézményi Tudásbázis-szerkezet szabványosított hello szervezeten belül.

A szabványos helyekre szükséges dokumentumok és hello mappaszerkezet nyújtunk sablonok. A gyökérmappa-szerkezetében hello fájlokat, amelyik tartalmazza az adatok feltárása és a szolgáltatás kivonása kódját, és jegyezze fel, amelyek modell ismétlési rendezi. Ezek a sablonok megkönnyítik a csapat tagjai toounderstand dolgozott mások és tooadd új tagok tooteams. Könnyen tooview és frissítés dokumentum sablonok markdown formátumban. Sablonok tooprovide ellenőrzőlisták az alábbiakhoz hasonló fontos kérdések használata minden egyes projekt tooinsure hello probléma jól meghatározott, és a termékek felel meg a várt hello minőségi. Példák erre vonatkozóan:

- a projekt okleveles toodocument hello üzleti probléma és hello projekt hatóköre
- adatok toodocument hello struktúra és a nyers adatok hello jelentések
- modell jelentések toodocument hello származtatott szolgáltatások
- modell metrikák: ROC görbék vagy MSE például


![TDSP-könyvtárak](./media/data-science-process-overview/tdsp-dir-structure.png)

hello könyvtárstruktúrát a klónozható [Github](https://github.com/Azure/Azure-TDSP-ProjectTemplate).

## <a name="infrastructure-and-resources-for-data-science-projects"></a>Infrastruktúra- és tudományos projektekhez erőforrások

TDSP megosztott elemzés és a tárolási infrastruktúrák kezelése ajánlásokat:

- felhő fájlrendszereket, olyan adatkészletek tárolása 
- adatbázisok
- big Data típusú adatok (Hadoop vagy külső) fürtök 
- Machine learning szolgáltatás. 

hello felhőalapú vagy helyszíni hello elemzés és tároló-infrastruktúra lehet. Ez a nyers és feldolgozott adatkészletek tárolására. Ez az infrastruktúra lehetővé teszi, hogy reprodukálható elemzés. Is elkerülhető a másolást, ami tooinconsistencies és szükségtelen infrastruktúra fenntartási költségeit. Eszközök megadott tooprovision hello megosztott erőforrások, nyomon követése, és biztonságosan lehetővé teszi minden team tag tooconnect-toothose erőforrások vannak. Akkor is célszerű rendelkezik projekt tagokat hozzon létre egy egységes számítási környezetet. Különböző tagja majd replikálja, és kísérletek ellenőrzése.

Íme egy példa egy csoport több projekten dolgozik, és különböző felhőalapú analytics infrastruktúra összetevőinek megosztása.

![TDSP-infrastruktúra](./media/data-science-process-overview/tdsp-analytics-infra.png)


## <a name="tools-and-utilities-for-project-execution"></a>Eszközöket és segédprogramokat projekt végrehajtása

A legtöbb szervezetben folyamatok bevezetéséről kihívást. A megadott eszközök tooimplement hello tudományos folyamata és életciklus súgó alacsonyabb hello korlátok tooand növelése hello konzisztenciájának azok elfogadását. TDSP tartalmaz egy kezdeti eszközeinek és parancsfájljainak toojump indítási elfogadását TDSP belül egy csoportnak. Emellett segít automatizálható néhány gyakori feladatok hello hello adatok tudományos életciklusának például az adatok feltárása és alapterv modellezési. Nincs jól definiált szerkezetben megadott egyéni toocontribute megosztott eszközök és segédprogramok történő a csoport megosztott kód tárházba. Más projektek belül hello csapat vagy szervezet hello majd is javítható ezeket az erőforrásokat. TDSP is tervezi az eszközök és segédprogramok toohello teljes Közösség tooenable hello hozzájárulásokat. a klónozható hello TDSP segédprogramok [Github](https://github.com/Azure/Azure-TDSP-Utilities).


## <a name="next-steps"></a>Következő lépések

[Team tudományos folyamata: Szerepköröket és feladatokat](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/roles-tasks.md) hello kulcsfontosságú személyzet szerepkörök és a kapcsolódó feladatok a folyamattal szabványosítja adatok tudományos csoport ismerteti. 
