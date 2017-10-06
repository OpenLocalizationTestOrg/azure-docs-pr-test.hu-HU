---
title: "aaaLearn hogyan toouse hello Twitter-összekötőt a logic apps |} Microsoft Docs"
description: "A REST API paraméterekkel Twitter-összekötő áttekintése"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 8bce2183-544d-4668-a2dc-9a62c152d9fa
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: ead4e4dc95bf894fd72b908c5375b407ba27642d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-twitter-connector"></a>Hello Twitter-összekötő az első lépései
Hello Twitter-összekötő segítségével:

* Tweeteket és get Twitter-üzenetek
* Hozzáférés ütemtervek, ismerősei és followers
* Hajtsa végre hello más műveletek és az alább ismertetett eseményindítók  

toouse [a csatlakozókat](apis-list.md), először toocreate logikai alkalmazás. Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/logic-apps-create-a-logic-app.md).  

## <a name="connect-tootwitter"></a>Csatlakozás tooTwitter
A Logic Apps alkalmazást férhetnek hozzá a szolgáltatáshoz, először jóvá kell toocreate egy *kapcsolat* toohello szolgáltatás. A [kapcsolat](connectors-overview.md) biztosít a logikai alkalmazás és egy másik szolgáltatás közötti kapcsolat.  

### <a name="create-a-connection-tootwitter"></a>Egy kapcsolat tooTwitter létrehozása
> [!INCLUDE [Steps toocreate a connection tooTwitter](../../includes/connectors-create-api-twitter.md)]
> 
> 

## <a name="use-a-twitter-trigger"></a>Twitter-eseményindítók
Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény. [További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

Ebben a példában I bemutatja, hogyan toouse hello **amikor egy új tweetet visszaküldi** toosearch #Seattle az indítás, és ha #Seattle megtalálható, frissítse a Dropbox fájlt hello tweetet hello szöveg. Vállalati például sikerült keresse meg a vállalat nevét hello és SQL-adatbázis hello tweetet hello szöveg frissíteni.

1. Adja meg *twitter* hello keresőmezőbe hello logic apps designer válassza hello **Twitter - amikor egy új tweetet visszaküldi** eseményindító   
   ![Twitter eseményindító kép 1](./media/connectors-create-api-twitter/trigger-1.png)  
2. Adja meg *#Seattle* a hello **keresett szöveg** vezérlő  
   ![Twitter eseményindító kép 2](./media/connectors-create-api-twitter/trigger-2.png) 

Ezen a ponton a Logic Apps alkalmazást az eseményindító, megkezdődik a hello futását más eseményindítók és műveletek hello munkafolyamat konfigurációja. 

> [!NOTE]
> A működési logic app toobe legalább egy eseményindító és egy műveletet kell tartalmaznia. Kövesse hello hello következő szakasz tooadd műveletet.  
> 
> 

## <a name="add-a-condition"></a>Feltétel hozzáadása
Mivel jelenleg csak a felhasználók több mint 50 felhasználóival Twitter-üzenetek iránt érdeklődik, olyan feltétel, amely megerősíti, hogy followers hello száma hozzáadása toohello logikai alkalmazást.  

1. Válassza ki **+ új lépés** tooadd hello művelet szeretné tootake Ha #Seattle tartalmaz egy új tweetet  
   ![Twitter művelet kép 1](../../includes/media/connectors-create-api-twitter/action-1.png)  
2. Jelölje be hello **feltétel hozzáadása** hivatkozásra.  
   ![Twitter feltétel kép 1](../../includes/media/connectors-create-api-twitter/condition-1.png)   
   Ekkor megnyílik a hello **feltétel** vezérlő, ahol ellenőrizheti a feltételek például *egyenlő*, *van kisebb, mint*, *nagyobb, mint*, *tartalmaz*stb.  
   ![Twitter feltétel kép 2](../../includes/media/connectors-create-api-twitter/condition-2.png)   
3. Jelölje be hello **adjon meg értéket** vezérlő.  
   A vezérlő választhat egy vagy több hello tulajdonságok bármely korábbi műveletek vagy eseményindítók hello akiknek állapota lesz kiértékelt tootrue vagy HAMIS értékként.
   ![Twitter feltétel kép 3](../../includes/media/connectors-create-api-twitter/condition-3.png)   
4. Jelölje be hello **...**  tooexpand hello tulajdonságlista hívjuk fel minden hello tulajdonságait.        
   ![Twitter feltétel kép 4](../../includes/media/connectors-create-api-twitter/condition-4.png)   
5. Jelölje be hello **Followers száma** tulajdonság.    
   ![Az állapot kép 5 Twitter](../../includes/media/connectors-create-api-twitter/condition-5.png)   
6. Figyelje meg hello Followers count tulajdonság már hello érték vezérlőben.    
   ![Twitter feltétel kép 6](../../includes/media/connectors-create-api-twitter/condition-6.png)   
7. Válassza ki **nagyobb, mint** hello operátorok listából.    
   ![Twitter feltétel kép 7](../../includes/media/connectors-create-api-twitter/condition-7.png)   
8. Itt adhatja meg 50 hello operandus hello *nagyobb, mint* operátor.  
   hello állapot jelenik meg. Mentse a munkáját, hello segítségével **mentése** hivatkozásra a fenti hello menüben.    
   ![Twitter feltétel kép 8](../../includes/media/connectors-create-api-twitter/condition-8.png)   

## <a name="use-a-twitter-action"></a>Egy Twitter művelettel
Egy művelet által definiált logikai alkalmazás hello munkafolyamat során. [További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

Most, hogy hozzáadta egy eseményindítót, kövesse ezeket a lépéseket tooadd egy műveletet, amely egy új tweetet hello Twitter-üzeneteket hello eseményindító által megtalált hello tartalmát fogja elküldeni. Ez az útmutató hello alkalmazásában csak Twitter-üzeneteket a felhasználók a több mint 50 followers lesznek közzétéve.  

Hello a következő lépésben adhat egy Twitter-műveletet, amely egy tweetet, az egyes tweetet, amelyek több mint 50 followers rendelkező felhasználó megtalálhatók hello tulajdonságok használatával fogja elküldeni.  

1. Válassza ki **művelet hozzáadása**. Ekkor megnyílik a kereshetnek, ahol a más műveletek és eseményindítók hello keresési vezérlőelemet.  
   ![Twitter feltétel kép 9](../../includes/media/connectors-create-api-twitter/condition-9.png)   
2. Adja meg *twitter* hello keresőmezőbe válassza hello **Twitter - egy tweetet utáni** művelet. Ekkor megnyílik a hello **egy tweetet utáni** szabályozása, ahol lép az összes részletes az elküldött hello tweetet.      
   ![A művelet kép 1-5 Twitter](../../includes/media/connectors-create-api-twitter/action-1-5.png)   
3. Jelölje be hello **Tweetet szöveg** vezérlő. A korábbi műveletek és a hello logic app eseményindítók összes kimenetének is látható. Válasszon ezek egyikét sem, és használatuk hello tweetet szövege hello új tweetet részeként.     
   ![Twitter művelet kép 2](../../includes/media/connectors-create-api-twitter/action-2.png)   
4. Válassza ki **felhasználónév**   
5. Adja meg *szereplő ellenőrzőösszeg:* hello tweetet szöveg vezérlőben. Ehhez a felhasználói név után.  
6. Válassza ki *Tweetet szöveg*.       
   ![Twitter művelet kép 3](../../includes/media/connectors-create-api-twitter/action-3.png)   
7. Mentse a munkáját, és küldjön egy tweetet hello #Seattle hashtaggel történő tooactivate rendelkező, a munkafolyamat.  


## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/twitterconnector/). 

## <a name="next-steps"></a>Következő lépések
[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md)

