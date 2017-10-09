---
title: "A Stream Analytics: Elforgatása napló hitelesítő adataival bemenetekhez és kimenetekhez |} Microsoft Docs"
description: "Ismerje meg, hogyan tooupdate hello hitelesítő adatait, a Stream Analytics bemenetekhez és kimenetekhez."
keywords: "a bejelentkezési hitelesítő adatok"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 42ae83e1-cd33-49bb-a455-a39a7c151ea4
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: ac2374c539012b66ab390656c5750024e02f6bdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a>Forgassa el a be- és kimenetekkel a Stream Analytics-feladatok bejelentkezési hitelesítő adatok
## <a name="abstract"></a>Absztrakt
Az Azure Stream Analytics jelenleg nem engedélyezi egy bemeneti/kimeneti hello hitelesítő adatok cseréjének hello feladat futása közben.

Azure Stream Analytics nem támogatja az utolsó kimenetből egy feladat folytatása, amíg meg akartunk tooshare hello teljes folyamatot a minimalizálja a hello lag hello leállítása és hello feladat elindítása és elforgatása hello bejelentkezési hitelesítő adatok között.

## <a name="part-1---prepare-hello-new-set-of-credentials"></a>1. rész – hello új hitelesítőadat-készletet előkészítése:
Ebben a részben a következő bemenetek/kimenetek alkalmazható toohello:

* Blob Storage
* Event Hubs
* SQL Database
* Table Storage

Az egyéb bemenetek/kimenetek folytassa a 2. rész.

### <a name="blob-storagetable-storage"></a>A BLOB storage-tároló vagy tábla
1. Nyissa meg a toohello tárolási kiterjesztés hello Azure Management Portal:  
   ![graphic1][graphic1]
2. Keresse meg a feladat által használt hello tárolási, és lépjen be:  
   ![graphic2][graphic2]
3. Kattintson a hello elérési kulcsok kezelése parancsot:  
   ![graphic3][graphic3]
4. Hello elsődleges elérési kulcsot és a másodlagos elérési kulcsot, hello közötti **válasszon egyet a feladat nem használja hello**.
5. Találati újragenerálása:  
   ![graphic4][graphic4]
6. Másolja az újonnan létrehozott hello kulcsot:  
   ![graphic5][graphic5]
7. Továbbra is tooPart 2.

### <a name="event-hubs"></a>Az Event hubs
1. Nyissa meg a toohello Service Bus bővítmény hello Azure Management Portal:  
   ![graphic6][graphic6]
2. Keresse meg a Service Bus Namespace a feladat által használt hello, és lépjen be:  
   ![graphic7][graphic7]
3. Ha a feladat egy megosztott elérési házirendet használ a Service Bus Namespace hello, jump toostep 6  
4. Nyissa meg toohello Event Hubs lapon:  
   ![graphic8][graphic8]
5. Keresse meg a feladat által használt Eseményközpont hello, és lépjen be:  
   ![graphic9][graphic9]
6. Nyissa meg toohello konfigurálása lapon:  
   ![graphic10][graphic10]
7. Házirend neve legördülő hello keresse meg a feladat által használt megosztott hello házirend:  
   ![graphic11][graphic11]
8. Hello elsődleges kulcs és a másodlagos kulcs hello közötti **válasszon egyet a feladat nem használja hello**.  
9. Találati újragenerálása:  
   ![graphic12][graphic12]
10. Másolja az újonnan létrehozott hello kulcsot:  
   ![graphic13][graphic13]
11. Továbbra is tooPart 2.  

### <a name="sql-database"></a>SQL Database
> [!NOTE]
> Megjegyzés: szüksége lesz a tooconnect toohello SQL adatbázis-szolgáltatás. Tooshow hogyan toodo a használatával hello egyszerűbben kezelhető a hello Azure felügyeleti portálon, de úgy is dönthet, toouse egy ügyféloldali eszközzel például SQL Server Management Studio alkalmazást, valamint fogjuk.
>
> 

1. Nyissa meg a toohello SQL-adatbázis bővítmény hello Azure Management Portal:  
   ![graphic14][graphic14]
2. Keresse meg a feladat által használt SQL-adatbázis hello és **hello kiszolgálón kattintson** menüjének hello azonos sorban:  
   ![graphic15][graphic15]
3. Kattintson a hello kezelése parancsot:  
   ![graphic16][graphic16]
4. Adatbázis fő típusa:  
   ![graphic17][graphic17]
5. Írja be a felhasználónevét és jelszavát, és kattintson a napló a:  
   ![graphic18][graphic18]
6. Kattintson az új lekérdezés:  
   ![graphic19][graphic19]
7. Típus a következő a lekérdezést a < login_name > cseréje a felhasználónévvel és lecserél hello <enterStrongPasswordHere> az új jelszóval:  
   `CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8. Kattintson a Futtatás gombra:  
   ![graphic20][graphic20]
9. Lépjen vissza toostep 2, és most kattintson hello adatbázis:  
   ![graphic21][graphic21]
10. Kattintson a hello kezelése parancsot:  
   ![graphic22][graphic22]
11. Írja be a felhasználónevét és jelszavát, és kattintson a bejelentkezés:  
   ![graphic23][graphic23]
12. Kattintson az új lekérdezés:  
   ![graphic24][graphic24]
13. Írja be a következő a lekérdezés < felhasználónév > cseréje egy névvel, amellyel tooidentify kíván hello ehhez a bejelentkezéshez, az adatbázis környezetében hello (biztosíthat hello ugyanazt az értéket < login_name >, például adott) és < login_name > cseréje az új felhasználónevet:  
   `CREATE USER <user_name> FROM LOGIN <login_name>`
14. Kattintson a Futtatás gombra:  
   ![graphic25][graphic25]
15. Most kell adni az új felhasználó hello azonos szerepkörrel és az eredeti felhasználó rendelkezik-e jogosultsággal.
16. Továbbra is tooPart 2.

## <a name="part-2-stopping-hello-stream-analytics-job"></a>2. lépés: Leállításával hello Stream Analytics-feladat
1. Nyissa meg a toohello Stream Analytics bővítmény hello Azure Management Portal:  
   ![graphic26][graphic26]
2. Keresse meg a feladat, és lépjen be:  
   ![graphic27][graphic27]
3. Nyissa meg a toohello bemeneti adatokat vagy hello kimenetek lapon e hello hitelesítő adatok bemenettel vagy kimenettel vannak elforgatása alapján.  
   ![graphic28][graphic28]
4. Kattintson a hello leállítási parancsot, és ellenőrizze a hello feladat leállt:  
   ![graphic29][graphic29] hello feladat toostop várja.
5. Keresse meg a kívánt toorotate hitelesítő adatok meg, és nyissa meg azt az hello-bemeneti/kimeneti:  
   ![graphic30][graphic30]
6. A folytatáshoz tooPart 3.

## <a name="part-3-editing-hello-credentials-on-hello-stream-analytics-job"></a>3. rész: Szerkesztés hello-felhasználó hitelesítő adatai hello Stream Analytics-feladat
### <a name="blob-storagetable-storage"></a>A BLOB storage-tároló vagy tábla
1. Hello Tárfiók kulcsa mező található, és az újonnan létrehozott kulcs beillesztése:  
   ![graphic31][graphic31]
2. Hello a Mentés gombra, és ellenőrizze, hogy a módosítások mentése:  
   ![graphic32][graphic32]
3. A kapcsolat tesztelése automatikusan elindul, amikor menti a módosításokat, győződjön meg arról, amely sikeresen megfelelt.
4. A folytatáshoz tooPart 4.

### <a name="event-hubs"></a>Az Event hubs
1. Hello Event Hub házirend kulcs mezőjében keresse meg és az újonnan létrehozott kulcs beillesztése:  
   ![graphic33][graphic33]
2. Hello a Mentés gombra, és ellenőrizze, hogy a módosítások mentése:  
   ![graphic34][graphic34]
3. A kapcsolat tesztelése automatikusan elindul, amikor menti a módosításokat, győződjön meg arról, hogy azt sikeresen megfelelt.
4. A folytatáshoz tooPart 4.

### <a name="power-bi"></a>Power BI
1. Kattintson a megújítás engedélyezési hello:  

   ![graphic35][graphic35]
2. A következő megerősítő hello jelenik meg:  

   ![graphic36][graphic36]
3. Hello a Mentés gombra, és ellenőrizze, hogy a módosítások mentése:  
   ![graphic37][graphic37]
4. A kapcsolat tesztelése automatikusan elindul, amikor a módosítások mentése, győződjön meg arról, hogy sikeresen megfelelt.
5. A folytatáshoz tooPart 4.

### <a name="sql-database"></a>SQL Database
1. Hello felhasználónév és a jelszó mező található, és illessze be az újonnan létrehozott hitelesítőadat-készletet a őket:  
   ![graphic38][graphic38]
2. Hello a Mentés gombra, és ellenőrizze, hogy a módosítások mentése:  
   ![graphic39][graphic39]
3. A kapcsolat tesztelése automatikusan elindul, amikor menti a módosításokat, győződjön meg arról, hogy azt sikeresen megfelelt.  
4. A folytatáshoz tooPart 4.

## <a name="part-4-starting-your-job-from-last-stopped-time"></a>4. lépés: A feladat kiindulva leállított legutóbbi
1. Hello bemeneti/kimeneti elhagyni:  
   ![graphic40][graphic40]
2. Kattintson a hello indítási parancsot:  
   ![graphic41][graphic41]
3. Válassza ki a hello feladat utolsó befejezési időpontja, és kattintson az OK gombra:  
   ![graphic42][graphic42]
4. A folytatáshoz tooPart 5.  

## <a name="part-5-removing-hello-old-set-of-credentials"></a>5. lépés: Eltávolítása hello régi hitelesítőadat-készletet
Ebben a részben a következő bemenetek/kimenetek alkalmazható toohello:

* Blob Storage
* Event Hubs
* SQL Database
* Table Storage

### <a name="blob-storagetable-storage"></a>A BLOB storage-tároló vagy tábla
Ismételje meg az 1. rész hello hozzáférési kulcsot a feladat által korábban használt toorenew hello most már nem használt a hozzáférési kulcsot.

### <a name="event-hubs"></a>Az Event hubs
Ismételje meg az 1. rész hello a feladat által korábban használt kulcs toorenew hello most már nem használt kulcs.

### <a name="sql-database"></a>SQL Database
1. Lépjen vissza toohello lekérdezési ablakba rész 1 7. lépés, és írja be a következő lekérdezést, < previous_login_name > lecserélését hello felhasználónevet, a feladat által korábban használt hello:  
   `DROP LOGIN <previous_login_name>`  
2. Kattintson a Futtatás gombra:  
   ![graphic43][graphic43]  

A következő megerősítő hello szerezheti be: 

    Command(s) completed successfully.

## <a name="get-help"></a>Segítségkérés
További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png

