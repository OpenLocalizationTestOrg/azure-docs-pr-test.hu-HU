---
title: aaaWhat a Microsoft Power BI Embedded?
description: "A Power BI Embedded lehetővé teszi toointegrate Power BI-jelentések a webes vagy mobilalkalmazás így nem kell egyéni megoldások toobuild."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 03649b72-b7d7-40ca-b077-12356d72d4f3
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/20/2017
ms.author: asaxton
ms.openlocfilehash: 0353938b6cdd9bb58b123b250f45f76b8cc7abe6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-microsoft-power-bi-embedded"></a>Újdonságok a Microsoft Power BI Embedded?
A **Power BI Embedded**, akkor integrálható a Power BI-jelentéseket jobbra a webhelyen vagy az alkalmazásokat.

![](media/powerbi-embedded-whats-is/what-is.png)

A Power BI Embedded van egy **Azure szolgáltatás** , amely lehetővé teszi az ISV-k, és az alkalmazásokon belül élményének app fejlesztők toosurface Power BI-adatokhoz. Fejlesztőként alkalmazások létrehozott, és ezeket az alkalmazásokat a saját felhasználók és a szolgáltatások meghatározott készletét rendelkezik. Ezek az alkalmazások is megtörténhet, néhány beépített elemek, például diagramokat és jelentéseket, amelyek a Microsoft Power BI Embedded által most állítható toohave. A Power BI-fiók toouse az alkalmazás nem szükséges. Akkor is továbbra is toosign tooyour alkalmazásban a korábbiakhoz hasonlóan, és megtekintésének és módosításának hello Power BI jelentéskészítési élmény anélkül, hogy további licencelést.

## <a name="licensing-for-microsoft-power-bi-embedded"></a>A licencelés Microsoft Power BI Embedded
A hello **Microsoft Power BI Embedded** használati modell, a Power BI nem hello végfelhasználói hello felelőssége licencelési.  Ehelyett **munkamenetek** hello fejlesztő hello alkalmazás, amely nem használ-e hello látványelemek vásárolt, és ezeket az erőforrásokat birtokló toohello előfizetés van szó. További információ a hello [árképzést ismertető oldalra](https://azure.microsoft.com/en-us/pricing/details/power-bi-embedded/).

## <a name="microsoft-power-bi-embedded-conceptual-model"></a>Microsoft Power BI Embedded fogalmi modellt

![](media/powerbi-embedded-whats-is/model.png)

Mint minden más szolgáltatáshoz, az Azure-ban, a Power BI Embedded erőforrásait törlődnek az hello [Azure Resource Manager API-k](https://msdn.microsoft.com/library/mt712306.aspx). Ebben az esetben hello erőforrás van egy **Power BI-Munkaterületcsoport**.

## <a name="workspace-collection"></a>Munkaterület-csoport
A **munkaterület-csoportok** hello legfelső szintű Azure tárolók erőforrásokat tartalmazó, 0 vagy több **munkaterületek**.  A **munkaterület** **gyűjtemény** rendelkezik az összes hello standard Azure-tulajdonság, valamint hello következő:

* **Hívóbetűk** – biztonságosan meghívásakor használt kulcsok hello Power BI API-k (ezt egy későbbi szakasz ismerteti).
* **Felhasználók** – Azure Active Directory (AAD) felhasználók rendszergazdai jogosultságokkal toomanage rendelkező Power BI-Munkaterületcsoport hello hello Azure-portálon vagy az Azure Resource Manager API használatával.
* **A régióban** – létesítési részeként egy **munkaterület-csoportok**, kiválaszthatja egy régió toobe kiépítve. További információkért lásd: [Azure-régiókat](https://azure.microsoft.com/regions/).

## <a name="workspace"></a>Munkaterület
A **munkaterület** egy olyan tároló, a Power BI tartalom, ilyenek például adatkészleteket és jelentéseket. A **munkaterület** üres létrehozásakor. Meg fogja írni a tartalom a Power BI Desktop segítségével, és szoftveres üzembe hello pbix-fájlt a munkaterületre hello segítségével [Power BI importálási API](https://msdn.microsoft.com/library/mt711504.aspx). A dataset programozott módon is létrehozhat és majd hozza létre a jelentéseket a Power BI Desktop használata helyett az alkalmazáson belül.

## <a name="using-workspace-collections-and-workspaces"></a>Munkaterület gyűjtemények és munkaterületek
**Munkaterület gyűjtemények** és **munkaterületek** olyan tárolók, amely használja, és bármelyik legjobb módja illik az Ön által létrehozott hello alkalmazás hello tervéhez rendezve tartalom. Számos különböző módja, hogy sikerült-e elrendezése bennük hello tartalom lesz. Úgy is dönthet, tooput egy munkaterületén található összes tartalmat, és majd későbbi használat app jogkivonatok toofurther tovább hello tartalom az ügyfelek között. Dönthet úgy is tooput összes, az ügyfelek a külön munkaterületek úgy, hogy néhány elkülönítése. Vagy régiónként, nem pedig ügyfél tooorganize felhasználók úgy is dönthet. Ez a rugalmas kialakítás teszi lehetővé toochoose hello legjobb módja tooorganize tartalom.

## <a name="cached-datasets"></a>Gyorsítótárazott adatkészletek
Gyorsítótárazott adatkészlet is használható.  Azonban nem lehet frissíteni a gyorsítótárazott adatokat, ha rendelkezik lett betöltve a **Microsoft Power BI Embedded**. A gyorsítótárazott dataset azt jelenti, hogy importálta hello adatokat a Power BI Desktop DirectQuery használata helyett.

## <a name="authentication-and-authorization-with-app-tokens"></a>Hitelesítési és engedélyezési az alkalmazási jogkivonatok
**Microsoft Power BI Embedded** tooyour alkalmazás tooperform eltér az összes hello szükséges felhasználói hitelesítési és engedélyezési. Nincs olyan explicit követelmény, hogy a végfelhasználók számára legyen-e a felhasználók az Azure Active Directory (Azure AD).  Ehelyett az alkalmazás túl kifejezze**Microsoft Power BI Embedded** engedélyezési toorender a Power BI-jelentés **alkalmazás a hitelesítési tokenek (alkalmazási jogkivonatok)**.  Ezek **alkalmazási jogkivonatok** jönnek létre, amikor az alkalmazás toorender jelentést szeretne igény szerint.

![](media/powerbi-embedded-whats-is/app-tokens.png)

**Alkalmazás a hitelesítési tokenek (alkalmazási jogkivonatok)** elleni használt tooauthenticate vannak **Microsoft Power BI Embedded**.  Három típusú **alkalmazási jogkivonatok**:

1. Jogkivonatok - felhasznált létesítésekor egy új létesítési **munkaterület** be egy **munkaterület-csoport**
2. Fejlesztői jogkivonatokat - használható, ha elvégzése hívja közvetlenül toohello **Power BI REST API-k**
3. Hello a jelentés beágyazott jogkivonatok - hívások toorender használni beágyazása iframe

Ezeket a jogkivonatokat használják hello ügyfélkapcsolati különböző fázisait **Microsoft Power BI Embedded**.  hello tokenek célja, hogy az alkalmazás tooPower BI a delegálhatóak engedélyek. További információkért lásd: [App Token Flow](power-bi-embedded-app-token-flow.md).

## <a name="create-or-edit-reports-within-your-application"></a>Hozzon létre vagy szerkeszt jelentéseket az alkalmazáson belül

Most Szerkesztés jelentések létezik, vagy hozzon létre új jelentéseket közvetlenül az alkalmazás anélkül, hogy a Power BI Desktop toouse is. Ehhez szükséges, hogy létezik-e a DataSet adatkészlet belül a munkaterületen.

## <a name="see-also"></a>Lásd még:

[Microsoft Power BI Embedded gyakori helyzetek](power-bi-embedded-scenarios.md)  
[Ismerkedés a Microsoft Power BI Embedded](power-bi-embedded-get-started.md)  
[Bevezetés a minta használatába](power-bi-embedded-get-started-sample.md)  
[Jelentés beágyazása](power-bi-embedded-embed-report.md)  
[Hitelesítés és engedélyezés a Power BI Embedded használatával](power-bi-embedded-app-token-flow.md)  
[JavaScript beágyazási minta](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[A csharp nyelvű Power bi Git-tárház](https://github.com/Microsoft/PowerBI-CSharp)  
[Power bi-csomópont Git-tárház](https://github.com/Microsoft/PowerBI-Node)  
További kérdései vannak? [Próbálja meg a Power BI-Közösség hello](http://community.powerbi.com/)
