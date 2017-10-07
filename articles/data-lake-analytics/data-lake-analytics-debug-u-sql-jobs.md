---
title: aaaDebug U-SQL feladatok |} Microsoft Docs
description: "Ismerje meg, hogyan nem sikerült a toodebug egy U-SQL Visual Studio használatával csúcspont."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: bcd0b01e-1755-4112-8e8a-a5cabdca4df2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/02/2016
ms.author: saveenr
ms.openlocfilehash: 092bffa1a59ed91c5837402d0276447480b923fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a>Felhasználó által definiált C# programkódja sikertelen U-SQL feladatok hibakeresése

U-SQL egy bővíthetőségi modell használatával C#, így a kód tooadd funkciókat, például egy egyéni készülék vagy nyomáscsökkentő írhat biztosít. több, lásd: toolearn [U-SQL programozástámogatási útmutató](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf). A gyakorlatban kódok hibakeresés esetleg, és a big data-rendszereket csupán korlátozott futási hibakeresési információk, például a naplófájlok.

Az Azure Data Lake Tools for Visual Studio nevű funkcióval rendelkezik **csúcspont Debug nem sikerült**, amely lehetővé teszi a sikertelen feladat a hello felhő tooyour helyi számítógépről hibakeresési klónozza. helyi másolat hello hello teljes felhőkörnyezetben, beleértve a bemeneti adatok és a felhasználói kód rögzíti.

hello következő videó bemutatja csúcspont Debug nem sikerült Azure Data Lake Tools for Visual Studio.

> [!VIDEO https://e0d1.wpc.azureedge.net/80E0D1/OfficeMixProdMediaBlobStorage/asset-d3aeab42-6149-4ecc-b044-aa624901ab32/b0fc0373c8f94f1bb8cd39da1310adb8.mp4?sv=2012-02-12&sr=c&si=a91fad76-cfdd-4513-9668-483de39e739c&sig=K%2FR%2FdnIi9S6P%2FBlB3iLAEV5pYu6OJFBDlQy%2FQtZ7E7M%3D&se=2116-07-19T09:27:30Z&rscd=attachment%3B%20filename%3DDebugyourcustomcodeinUSQLADLA.mp4]
>

> [!NOTE]
> A Visual Studio a következő két frissítések hello van szükség, ha még nincs telepítve: [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) és a [Universal C futásidejű for Windows](https://www.microsoft.com/download/details.aspx?id=50410).

## <a name="download-failed-vertex-toolocal-machine"></a>Nem sikerült letölteni a csúcspont toolocal gép

Amikor megnyit sikertelen feladat a Azure Data Lake Tools for Visual Studio, egy sárga értesítési sáv hello hiba lapon a részletes hibaüzenetek látható.

1. Kattintson a **letöltése** összes hello toodownload szükséges erőforrások és a bemeneti adatfolyam. Ha hello letöltési indul el, kattintson a **újra**.

2. Kattintson a **nyitott** hello letöltés toogenerate helyi hibakeresési környezetben befejezése után. Az automatikusan létrehozott és megnyitott egy új Visual Studio-példány és a hibakeresési megoldás.

![Az Azure Data Lake Analytics U-SQL hibakeresési visual studio letöltése csúcspont](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

Feladatok közé tartozhat a háttérkód forrásfájlok vagy a regisztrált szerelvényeket, és két különböző hibakeresési forgatókönyvek rendelkezik.

- [Sikertelen feladat a háttérkód hibakeresése](#debug-job-failed-with-code-behind)
- [Sikertelen feladat a hibakereséshez szerelvények](#debug-job-failed-with-assemblies)


## <a name="debug-job-failed-with-code-behind"></a>Feladat sikertelen volt a háttérkód hibakeresése

Ha egy U-SQL-feladat sikertelen lesz, és hello feladat tartalmazza a felhasználói kód (általában nevű `Script.usql.cs` U-SQL projekt), hogy forráskód megoldás hibakeresés hello importálta-e.  Innen hello Visual Studio hibakeresési eszközök (figyelési, változók stb.) tootroubleshoot hello probléma is használhatja.

> [!NOTE]
> Előtt hibakeresés, lehet, hogy toocheck **közös nyelvi futtatókörnyezet kivételek** hello kivétel beállításai ablakban (**Ctrl + Alt + E**).

![Az Azure Data Lake Analytics U-SQL hibakeresési visual studio-beállítás](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

1. Nyomja le az **F5** toorun hello háttérkód kódot. Akkor fog futni, amíg egy kivétel miatt leállt.

2. Nyissa meg hello `ADLTool_Codebehind.usql.cs` fájlt, és állítson be töréspontokat, majd nyomja le az **F5** toodebug hello kód lépésről lépésre.

    ![Az Azure Data Lake Analytics U-SQL hibakeresési kivétel](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

## <a name="debug-job-failed-with-assemblies"></a>Feladat sikertelen volt a szerelvények hibakeresését

Ha regisztrált szerelvényeket a U-SQL parancsfájlt használja, hello rendszer automatikusan hello forráskód nem lehet lekérdezni. Ebben az esetben manuálisan adja hozzá hello szerelvények forrás kód fájlok toohello megoldás.

### <a name="configure-hello-solution"></a>Hello megoldás konfigurálása

1. Kattintson a jobb gombbal **megoldás "VertexDebug" > vegye fel > létező projekt...**  toofind forráskód szerelvényeket hello, és adja hozzá a hello projekt toohello megoldás hibakeresést.

    ![Az Azure Data Lake Analytics U-SQL hibakeresési projekt hozzáadása](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. Kattintson a jobb gombbal **LocalVertexHost > Tulajdonságok** a hello megoldás, és másolja hello **munkakönyvtár** elérési útja.

3. Kattintson a jobb gombbal **forrás kód szerelvényprojektet > Tulajdonságok**, jelölje be hello **Build** bal oldali lapon, és illessze be a másolt hello elérési útjára **kimeneti > elérési utat a kimeneti**.

    ![Az Azure Data Lake Analytics U-SQL hibakeresési pdb elérési útjának beállítása](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

4. Nyomja le az **Ctrl + Alt + E**, ellenőrizze **közös nyelvi futtatókörnyezet kivételek** kivétel beállításai ablakban.

### <a name="start-debug"></a>Indítsa el a hibakeresési

1. Kattintson a jobb gombbal **szerelvény forráskód Projekt > újraépítése** toooutput .pdb fájlok toohello `LocalVertexHost` munkakönyvtárát.

2. Nyomja le az **F5** és hello projekt fog futni, amíg egy kivétel miatt leállt. A következő figyelmeztető üzenet, amely biztonságosan figyelmen kívül hagyhatja hello jelenhet meg. Tooa perces tooget toohello hibakeresési képernyő is eltarthat.

    ![Az Azure Data Lake Analytics U-SQL hibakeresési visual studio figyelmeztetés](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

3. Nyissa meg a forráskódot, és állítson be töréspontokat, majd nyomja le az **F5** toodebug hello kód lépésről lépésre.

Hello Visual Studio hibakeresési eszközök (figyelési, változók stb.) tootroubleshoot hello probléma is használható.

> [!NOTE]
> Hello szerelvény forrás kód projekt újraépítéséhez hello kód frissítése toogenerate .pdb fájlok módosítása után minden alkalommal.

Hibakeresés, miután hello projekt fejeződik hello kimeneti ablakban látható a következő üzenet hello:

```
hello Program 'LocalVertexHost.exe' has exited with code 0 (0x0).
```

![Az Azure Data Lake Analytics U-SQL hibakeresési sikeres](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

## <a name="resubmit-hello-job"></a>Küldje el újra a hello feladat

Ha befejezte a hibakeresést, küldje el újra a hello sikertelen feladat adatait.

1. A feladatok a háttérkód megoldásokkal, másolja a C#-kódban hello háttérkód forrás fájlba (általában `Script.usql.cs`).
2. A feladatokat a szerelvényeket frissített hello .dll szerelvényeket a ADLA adatbázisba regisztrálása:
    1. A Server Explorer vagy a Cloud Explorerben bontsa ki a hello **ADLA fiók > adatbázisok** csomópont.
    2. Kattintson a jobb gombbal **szerelvények** , és regisztrálja az új .dll szerelvények hello ADLA adatbázissal: ![Azure Data Lake Analytics U-SQL hibakeresési szerelvény regisztrálása](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)
3. Küldje el újra a feladatot.

## <a name="next-steps"></a>Következő lépések

- [U-SQL programozástámogatási útmutató](data-lake-analytics-u-sql-programmability-guide.md)
- [Azure Data Lake Analytics-feladatok U-SQL-felhasználó által definiált operátorok fejlesztése](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [Oktatóanyag: Data Lake Tools for Visual Studio használatával U-SQL-parancsfájlok fejlesztése](data-lake-analytics-data-lake-tools-get-started.md)
