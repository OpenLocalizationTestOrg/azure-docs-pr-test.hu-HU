---
title: "aaaLearn hogyan toouse hello FTP-összekötőt a logic apps |} Microsoft Docs"
description: "Az Azure App service logic Apps alkalmazások létrehozása Csatlakozás tooFTP server toomanage a fájlokat. Például feltöltés különféle műveleteket hajtson végre, frissítése, lekérése és FTP-kiszolgáló fájlok törlése."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: d83c55fe-eb59-4b7b-a5ec-afac5c772616
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/22/2016
ms.author: mandia; ladocs
ms.openlocfilehash: a7020df2005ebb34fc569627ae0516b8528cc7a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-ftp-connector"></a>Hello FTP-összekötő az első lépései
Hello FTP összekötő toomonitor használja, kezeléséhez, és hozza létre az FTP-kiszolgálón fájlokat. 

toouse [a csatlakozókat](apis-list.md), először toocreate logikai alkalmazás. Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-tooftp"></a>Csatlakozás tooFTP
A Logic Apps alkalmazást férhetnek hozzá a szolgáltatáshoz, először jóvá kell toocreate egy *kapcsolat* toohello szolgáltatás. A [kapcsolat](connectors-overview.md) biztosít a logikai alkalmazás és egy másik szolgáltatás közötti kapcsolat.  

### <a name="create-a-connection-tooftp"></a>Egy kapcsolat tooFTP létrehozása
> [!INCLUDE [Steps toocreate a connection tooFTP](../../includes/connectors-create-api-ftp.md)]
> 
> 

## <a name="use-a-ftp-trigger"></a>FTP-eseményindítók
Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény. [További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

> [!IMPORTANT]
> hello FTP-összekötő számára szükséges, hello Internet elérhető-e, és az FTP-kiszolgáló konfigurálva toooperate passzív módra. Hello FTP összekötő is **implicit ftps-t (FTP szolgáltatás SSL-en keresztül) nem kompatibilis**. hello FTP-összekötő csak explicit ftps-t (FTP szolgáltatás SSL-en keresztül) támogatja.  
> 
> 

Ebben a példában I bemutatja, hogyan toouse hello **FTP - amikor egy fájl hozzáadása vagy módosítása** indul el egy logic app munkafolyamat tooinitiate, ha egy fájl hozzá vagy módosítja az FTP-kiszolgálóhoz. A vállalati tegyük fel használhat az eseményindító toomonitor egy FTP-mappa új fájlokat, amelyek megfelelnek a rendeléseket az ügyfelektől.  Az FTP-összekötő művelet aztán használhatja például a **fájl tartalmának lekérdezése** hello ahhoz, hogy további feldolgozás és a rendeléseket adatbázis tárolási tooget hello tartalmát.

1. Adja meg *ftp* hello keresőmezőbe hello logic apps designer válassza hello **FTP - amikor egy fájl hozzáadása vagy módosítása** eseményindító   
   ![FTP eseményindító kép 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)  
   Hello **amikor egy fájl hozzáadása vagy módosítása esetén** vezérlő megnyílik.  
   ![FTP eseményindító kép 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)  
2. Jelölje be hello **...**  hello vezérlőelem hello jobb oldalán található. Ekkor megnyílik a hello mappa példányválasztó vezérlő  
   ![FTP eseményindító kép 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)  
3. Jelölje be hello  **>**  (jobbra), és keresse meg a toofind hello mappa toomonitor szánt új vagy módosított fájlokat. Válasszon ki hello mappát, és figyelje meg, hello mappa megjelenik a hello **mappa** vezérlő.  
   ![FTP eseményindító kép 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)   

Ezen a ponton a Logic Apps alkalmazást van beállítva, amely akkor kezdődik, hello futását más eseményindítók és műveletek hello munkafolyamat amikor egy fájl módosítani vagy mappában létrehozott hello adott FTP eseményindító. 

> [!NOTE]
> A működési logic app toobe legalább egy eseményindító és egy műveletet kell tartalmaznia. Kövesse hello hello következő szakasz tooadd műveletet.  
> 
> 

## <a name="use-a-ftp-action"></a>Egy FTP művelettel
Egy művelet által definiált logikai alkalmazás hello munkafolyamat során. [További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

Most, hogy egy eseményindító jelentek meg, kövesse ezeket lépéseket tooadd egy műveletet, amely megkapja hello hello eseményindító által megtalált hello új vagy módosított fájl tartalmát.    

1. Válassza ki **+ új lépés** tooadd hello hello művelet tooget hello hello fájl tartalmának hello FTP-kiszolgálón  
2. Jelölje be hello **művelet hozzáadása** hivatkozásra.  
   ![FTP-művelet kép 1](./media/connectors-create-api-ftp/ftp-action-1.png)  
3. Adja meg *FTP* minden műveletre toosearch kapcsolódó tooFTP.
4. Válassza ki **FTP - fájl tartalmának lekérdezése** , hello művelet tootake, amikor egy új vagy módosított fájl hello FTP mappában található.      
   ![FTP-művelet kép 2](./media/connectors-create-api-ftp/ftp-action-2.png)  
   Hello **fájl tartalmának lekérdezése** megnyílik szabályozzák. **Megjegyzés:**: akkor lesz felszólító tooauthorize a logic app tooaccess az FTP-kiszolgáló fiókját, ha még nem meg korábban.  
   ![FTP-művelet kép 3](./media/connectors-create-api-ftp/ftp-action-3.png)   
5. Jelölje be hello **fájl** vezérlő (hello szóköz alatt található **fájl***). Itt akkor használhatja a hello különböző tulajdonságok fájlból hello új vagy módosított hello FTP-kiszolgálón található.  
6. Jelölje be hello **tartalom fájl** lehetőséget.  
   ![FTP-művelet kép 4](./media/connectors-create-api-ftp/ftp-action-4.png)   
7. hello vezérlő frissül, amely azt jelzi, hogy hello **FTP - fájl tartalmának lekérdezése** művelet kap hello *tartalom fájl* hello új vagy módosított fájl hello FTP-kiszolgálón.      
   ![FTP-művelet kép 5](./media/connectors-create-api-ftp/ftp-action-5.png)     
8. Mentse a munkáját, majd adja hozzá a fájl toohello FTP mappa tootest a munkafolyamat.    

Ezen a ponton hello logikai alkalmazás lett konfigurálva egy eseményindító toomonitor egy mappát egy FTP-kiszolgáló és a kezdeményezési hello munkafolyamat Ha talál egy új fájl vagy a módosított fájlt hello FTP-kiszolgálón. 

hello logikai alkalmazás is van konfigurálva legyen egy új vagy módosított fájl hello művelet tooget hello tartalmát.

Mostantól hozzáadhatja azok egy másik művelet, például hello [SQL Server - sor beszúrása](connectors-create-api-sqlazure.md) művelet tooinsert hello SQL-adatbázistáblában szereplő hello új vagy módosított fájl tartalmát.  

## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/ftpconnector/). 

## <a name="next-steps"></a>Következő lépések
[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md)

