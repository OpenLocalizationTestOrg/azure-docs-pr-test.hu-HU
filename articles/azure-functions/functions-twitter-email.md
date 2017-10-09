---
title: "egy függvény, amely az Azure Logic Apps aaaCreate |} Microsoft Docs"
description: "Hozzon létre egy függvényt, amely az Azure Logic Apps és az Azure kognitív szolgáltatások toocategorize tweetet véleményeket, és értesítést küld, ha véleményeket gyenge."
services: functions, logic-apps, cognitive-services
keywords: "munkafolyamat, felhőalapú alkalmazások, felhőszolgáltatások, üzleti folyamatok, rendszerintegráció, vállalati alkalmazásintegráció, EAI"
documentationcenter: 
author: ggailey777
manager: erikre
editor: 
ms.assetid: 60495cc5-1638-4bf0-8174-52786d227734
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: e176bd946af9a3684b3ad0e4b1bed1c3ee344019
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-that-integrates-with-azure-logic-apps"></a>Hozzon létre egy függvényt, amely az Azure Logic Apps

Az Azure Functions integrálja az Azure Logic Apps a Logic Apps Designer hello. Ez az integráció lehetővé teszi az informatika álló üzenettípusok összehangolását más Azure és a külső függvények power hello használatát. 

Az oktatóanyag bemutatja, hogyan toouse működik, a Twitter-bejegyzéseket a Logic Apps és az Azure kognitív szolgáltatások tooanalyze véleményeket. Az indított HTTP függvény kategorizálja Twitter-üzeneteket, mint a zöld, sárga vagy piros hello véleményeket pontszám alapján. Az e-mail elküldésekor történik a gyenge véleményeket észlelése esetén. 

![kép két lépést a Logic App Designer alkalmazás](media/functions-twitter-email/designer1.png)

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * Kognitív Services-fiók létrehozása.
> * Hozzon létre egy függvényt, Kategorizáló tweetet céggel kapcsolatos véleményeket.
> * TooTwitter csatlakozó logikai alkalmazás létrehozása.
> * Adja hozzá a céggel kapcsolatos véleményeket észlelési toohello logikai alkalmazást. 
> * Csatlakozás hello logic app toohello függvény.
> * Küldjön egy e-mailt hello válaszát hello függvény alapján.

## <a name="prerequisites"></a>Előfeltételek

+ Az aktív [Twitter](https://twitter.com/) fiók. 
+ Egy [Outlook.com-os](https://outlook.com/) fiókot (a értesítések küldése).
+ Ez a témakör használja, mint a kiindulási pont hello erőforrások létrehozott [hello Azure-portálon az első függvényét létrehozása](functions-create-first-azure-function.md).  
Ha még nem tette meg, végezze el ezeket a lépéseket most toocreate a függvény alkalmazást.

## <a name="create-a-cognitive-services-account"></a>Kognitív Services-fiók létrehozása

Egy kognitív Services-fiók szükséges toodetect hello véleményeket Twitter-üzeneteket figyeli a rendszer.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).

2. Kattintson a hello **új** hello bal felső sarkában hello Azure-portálon található gombra.

3. Kattintson a **adatok + analitika** > **kognitív szolgáltatások**. Majd, mint hello-beállítások használata hello táblázatban megadott hello fogadnia, és ellenőrizze **PIN-kód toodashboard**.

    ![Hozzon létre kognitív fiók panelen](media/functions-twitter-email/cog_svcs_account.png)

    | Beállítás      |  Ajánlott érték   | Leírás                                        |
    | --- | --- | --- |
    | **Name (Név)** | MyCognitiveServicesAccnt | Válasszon egy egyedi fióknevet. |
    | **API-típus** | Szövegelemzések API | API használt tooanalyze szöveg.  |
    | **Hely** | USA nyugati régiója | Jelenleg csak **USA nyugati régiója** szövegelemzések érhető el. |
    | **Tarifacsomag** | F0 | Első lépésként hello legalacsonyabb szint. Ha elfogy a hívásokat, méretezhető tooa magasabb szintű használható.|
    | **Erőforráscsoport** | myResourceGroup | Használja az azonos hello erőforráscsoport ebben az oktatóanyagban minden szolgáltatáshoz.|

4. Kattintson a **létrehozása** toocreate fiókját. Hello-fiók létrehozása után kattintson az új kognitív szolgáltatások fiók rögzített toohello irányítópulton. 

5. Hello fiókot, kattintson **kulcsok**, majd másolja az hello értékének **kulcs 1** és mentse azt. A kulcs tooconnect hello logic app tooyour kognitív Services-fiók használ. 
 
    ![Kulcsok](media/functions-twitter-email/keys.png)

## <a name="create-hello-function"></a>Hello függvény létrehozása

A Functions egy kiváló módja toooffload a logic apps munkafolyamat feladatokat. Ez az oktatóanyag egy indított HTTP függvény tooprocess tweetet véleményeket pontszámok kognitív szolgáltatások és a visszatérési kategória értéket használja.  

1. Bontsa ki a függvény app, kattintson a hello  **+**  gomb melletti túl**funkciók**, kattintson a hello **HTTPTrigger** sablont. Típus `CategorizeSentiment` hello függvény **neve** kattintson **létrehozása**.

    ![Függvény alkalmazások panelről, Funkciók +](media/functions-twitter-email/add_fun.png)

2. Cserélje le a következő kód hello hello hello run.csx fájl tartalmát, majd kattintson az **mentése**:

    ```c#
    using System.Net;
    
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        // hello sentiment category defaults too'GREEN'. 
        string category = "GREEN";
    
        // Get hello sentiment score from hello request body.
        double score = await req.Content.ReadAsAsync<double>();
        log.Info(string.Format("hello sentiment score received is '{0}'.",
                    score.ToString()));
    
        // Set hello category based on hello sentiment score.
        if (score < .3)
        {
            category = "RED";
        }
        else if (score < .6)
        {
            category = "YELLOW";
        }
        return req.CreateResponse(HttpStatusCode.OK, category);
    }
    ```
    Ez a függvény kód hello véleményeket pontszám hello kérés alapján szín kategória adja vissza. 

3. tootest hello függvény, kattintson a **teszt** hello jobb szélén tooexpand hello teszt lapját. Írjon be egy értéket a `0.2` a hello **Request body**, és kattintson a **futtatása**. Érték **piros** hello törzsét hello választ ad vissza. 

    ![Teszt hello függvényt hello Azure-portálon](./media/functions-twitter-email/test.png)

Most is a céggel kapcsolatos véleményeket pontszámok Kategorizáló működnek. Ezután hozzon létre egy logikai alkalmazás, amely a függvény a Twitter és kognitív szolgáltatások fiókokhoz. 

## <a name="create-a-logic-app"></a>Logikai alkalmazás létrehozása   

1. A hello Azure-portálon, kattintson a hello **új** hello bal felső sarkában hello Azure-portálon található gombra.

2. Kattintson a **vállalati integrációs** > **logikai alkalmazás**. Majd, mint hello-beállítások használata hello táblázatban megadott ellenőrizze **PIN-kód toodashboard**, és kattintson a **létrehozása**.
 
4. Írja be a **neve** például `TweetSentiment`, hello beállításokkal hello táblázatban megadottak szerint, hello fogadnia, és ellenőrizze **PIN-kód toodashboard**.

    ![Az Azure-portálon hello logikai alkalmazás létrehozása](./media/functions-twitter-email/new_logicApp.png)

    | Beállítás      |  Ajánlott érték   | Leírás                                        |
    | ----------------- | ------------ | ------------- |
    | **Name (Név)** | TweetSentiment | Válasszon egy megfelelő az alkalmazás nevét. |
    | **Erőforráscsoport** | myResourceGroup | API használt tooanalyze szöveg.  |
    | **Hely** | USA keleti régiója | Válasszon egy helyet Bezárás tooyou. |
    | **Erőforráscsoport** | myResourceGroup | Válassza ki azt korábban hello azonos meglévő erőforráscsoportot.|

4. Kattintson a **létrehozása** toocreate a Logic Apps alkalmazást. Hello alkalmazás létrehozása után kattintson az új logikai alkalmazás rögzített toohello irányítópulton. Majd hello Logic Apps Designer, görgessen lefelé, és kattintson a hello **üres logikai alkalmazás** sablont. 

    ![Üres Logic Apps-sablon](media/functions-twitter-email/blank.png)

Hello Logic Apps Designer tooadd szolgáltatások és eseményindítók tooyour alkalmazás most már használhatja.

## <a name="connect-tootwitter"></a>Csatlakozás tooTwitter

Először hozzon létre egy kapcsolat tooyour Twitter-fiók. hello logikai alkalmazás lekérdezi az Twitter-üzeneteket, amelyek hello app toorun indítható el.

1. Hello tervezőben kattintson hello **Twitter** szolgáltatásra, és kattintson a hello **amikor egy új tweetet visszaküldi** eseményindító. Jelentkezzen be tooyour Twitter-fiók, és engedélyezheti a Logic Apps toouse fiókját.

2. Beállításokkal hello Twitter eseményindító hello táblázatban megadottak szerint. 

    ![Twitter-összekötő beállításai](media/functions-twitter-email/azure_tweet.png)

    | Beállítás      |  Ajánlott érték   | Leírás                                        |
    | ----------------- | ------------ | ------------- |
    | **Keresett szöveg** | #Azure | Használja a hashtaggel történő, amely elegendő népszerű toogenerate új Twitter-üzeneteket hello választott időszakban. Hello ingyenes szint és a hashtaggel történő használata esetén túl népszerű gyorsan használhatja fel hello tranzakciók a kognitív Services-fiókhoz. |
    | **Gyakoriság** | Perc | a lekérdezés Twitter használt hello gyakoriság egység.  |
    | **Időköz** | 15 | Twitter-kérelmek gyakorisága egységekben között eltelt hello idő. |

3.  Kattintson a **mentése** tooconnect tooyour Twitter-fiók. 

Az alkalmazás most már csatlakoztatott tooTwitter áll. A következő csatlakozás tootext analytics toodetect hello véleményeket az összegyűjtött Twitter-üzeneteket.

## <a name="add-sentiment-detection"></a>Adja hozzá a céggel kapcsolatos véleményeket észlelése

1. Kattintson a **új lépés**, majd **művelet hozzáadása**.

    ![Új lépéssel, majd a művelet hozzáadása](media/functions-twitter-email/new_step.png)

2. A **művelet kiválasztását**, kattintson a **Szövegelemzések**, és kattintson a hello **észleli a céggel kapcsolatos véleményeket** művelet.

    ![Véleményeket észlelése](media/functions-twitter-email/detect_sent.png)

3. Írja be a kapcsolat neve, mint `MyCognitiveServicesConnection`, illessze be a hello kulcs, a mentett a kognitív szolgáltatások fiókra, majd kattintson **létrehozása**.  

4. Kattintson a **szöveg tooanalyze** > **Tweetet szöveg**, és kattintson a **mentése**.  

    ![Véleményeket észlelése](media/functions-twitter-email/ds_tta.png)

Most, hogy a céggel kapcsolatos véleményeket észlelési van konfigurálva, a kapcsolat tooyour függvény, amely akkor hello véleményeket pontszám kimeneti is hozzáadhat.

## <a name="connect-sentiment-output-tooyour-function"></a>Csatlakozás a céggel kapcsolatos véleményeket kimeneti tooyour függvény

1. A Logic Apps Designer hello, kattintson **új lépés** > **művelet hozzáadása**, és kattintson a **Azure Functions**. 

2. Kattintson a **jelöljön ki egy Azure függvényt**, jelölje be hello **CategorizeSentiment** korábban létrehozott függvény.  

    ![Válasszon egy Azure függvény megjelenítő Azure függvény mezőben](media/functions-twitter-email/choose_fun.png)

3. A **kérelem törzse**, kattintson a **pontszám** , majd **mentése**.

    ![Pontszám](media/functions-twitter-email/trigger_score.png)

Most a függvény lesz kiváltva, ha a céggel kapcsolatos véleményeket pontszám küldi hello logikai alkalmazást. Itt a színkódolás kategória hello függvény által visszaadott toohello logikai alkalmazást. Ezután adja hozzá az e-mailben értesítést küldi, ha a céggel kapcsolatos véleményeket érték **piros** hello függvény küld vissza. 

## <a name="add-email-notifications"></a>E-mail értesítések hozzáadása

hello hello munkafolyamat utolsó része esetén tootrigger egy e-mailt hello véleményeket pontozza a mennyiségeket _piros_. Ez a témakör egy Outlook.com-összekötőt használja. Hasonló lépéseket toouse Gmail vagy Office 365 Outlook összekötő végezheti el.   

1. A Logic Apps Designer hello, kattintson **új lépés** > **feltétel hozzáadása**. 

2. Kattintson a **adjon meg értéket**, majd kattintson a **törzs**. Válassza ki **egyenlő**, kattintson a **adjon meg értéket** és írja be `RED`, és kattintson a **mentése**. 

    ![Adjon hozzá egy feltétel toohello logikai alkalmazást.](media/functions-twitter-email/condition.png)

3. A **Ha igen, nincs**, kattintson a **művelet hozzáadása**, keresse meg `outlook.com`, kattintson **egy e-mailek küldése**, és jelentkezzen be tooyour Outlook.com-fiók.
    
    ![Hello feltétel művelet kiválasztását.](media/functions-twitter-email/outlook.png)

    > [!NOTE]
    > Ha egy Outlook.com-fiók nem rendelkezik, választhat egy másik összekötő, például a Gmailes vagy Office 365 Outlook

4. A hello **egy e-mailek küldése** művelet, mint hello e-mail beállításokat meg hello táblában. 

    ![Konfigurálhatja a hello küldési e-mail művelet hello e-maileket.](media/functions-twitter-email/sendemail.png)

    | Beállítás      |  Ajánlott érték   | Leírás  |
    | ----------------- | ------------ | ------------- |
    | **A** | Az e-mail címet | hello e-mail címet, amely hello értesítést kap. |
    | **Tulajdonos** | Negatív tweetet céggel kapcsolatos véleményeket észlelt  | hello hello e-mail értesítés tárgyát.  |
    | **Törzs** | Tweetet szöveg, amelyet helyre | Kattintson a hello **Tweetet szöveg** és **hely** paraméterek. |

5.  Kattintson a **Save** (Mentés) gombra.

Most, hogy hello munkafolyamat befejeződött, hello logikai alkalmazás engedélyezése, és tekintse meg a munkahelyi hello függvény.

## <a name="test-hello-workflow"></a>Teszt hello munkafolyamat

1. A Logic App Designer hello, kattintson **futtatása** toostart hello alkalmazást.

2. A hello bal oldali oszlopban kattintson **áttekintése** hello logikai alkalmazás toosee hello állapotát. 
 
    ![Logic app végrehajtási állapota](media/functions-twitter-email/over1.png)

3. (Választható) Kattintson az egyik hello futtatása toosee hello végrehajtási részleteit.

4. Nyissa meg tooyour függvény, hello naplók megtekintése, és győződjön meg arról, hogy a céggel kapcsolatos véleményeket értéket is fogad és dolgoz.
 
    ![Függvény naplók megtekintése](media/functions-twitter-email/sent.png)

5. A potenciálisan negatív véleményeket észlelésekor e-mailt kapni. Ha még nem kapott e-mailt, minden egyes hello függvény kód tooreturn piros lehet módosítani:

        return req.CreateResponse(HttpStatusCode.OK, "RED");

    Miután ellenőrizte, hogy e-mail értesítést, módosítsa a hátsó toohello eredeti kódot:

        return req.CreateResponse(HttpStatusCode.OK, category);

    > [!IMPORTANT]
    > Ez az oktatóanyag befejezése után le kell tiltania hello logikai alkalmazást. Hello alkalmazás letiltása, elkerülheti a által használt és a végrehajtások megterhelni hello tranzakciók a kognitív Services-fiók mentése folyamatban.

Most láthatta, milyen egyszerűen a Logic Apps munkafolyamat toointegrate funkcióit.

## <a name="disable-hello-logic-app"></a>Hello logikai alkalmazás letiltása

toodisable hello logic app, kattintson a **áttekintése** , majd **letiltása** üdvözlő képernyőt hello tetején. Ezzel leállítja a hello logikai alkalmazás fut, és ezzel járó költségek hello alkalmazás törlése nélkül. 

![Függvény naplók](media/functions-twitter-email/disable-logic-app.png)

## <a name="next-steps"></a>Következő lépések

Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Kognitív Services-fiók létrehozása.
> * Hozzon létre egy függvényt, Kategorizáló tweetet céggel kapcsolatos véleményeket.
> * TooTwitter csatlakozó logikai alkalmazás létrehozása.
> * Adja hozzá a céggel kapcsolatos véleményeket észlelési toohello logikai alkalmazást. 
> * Csatlakozás hello logic app toohello függvény.
> * Küldjön egy e-mailt hello válaszát hello függvény alapján.

Hogyan előzetes toohello következő útmutató toolearn toocreate egy kiszolgáló nélküli API-t, a függvény.

> [!div class="nextstepaction"] 
> [Kiszolgáló nélküli API létrehozása az Azure Functions használatával](functions-create-serverless-api.md)

További információk a Logic Apps toolearn lásd [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).

