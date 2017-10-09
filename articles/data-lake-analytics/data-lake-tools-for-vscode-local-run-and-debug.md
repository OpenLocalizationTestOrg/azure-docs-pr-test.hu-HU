---
title: "Az Azure Data Lake Tools: U-SQL helyi futtatásával és a Visual Studio Code helyi hibakeresési |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Data Lake Tools for Visual Studio Code toolocal lefutott, és helyi hibakeresése."
Keywords: "VScode, az Azure Data Lake Tools, a helyi futtatáskor, helyi hibakeresése a helyi debug preview tárolófájl feltöltése toostorage elérési útja"
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: DJ
editor: jejiang
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: big-data
ms.date: 07/14/2017
ms.author: jejiang
ms.openlocfilehash: fb152f07fe8c4b03dde8fb8e62c7475eccda0578
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="u-sql-local-run-and-local-debug-with-visual-studio-code"></a>U-SQL helyi futtatásával és a helyi hibakeresése a Visual Studio Code

## <a name="prerequisites"></a>Előfeltételek
Győződjön meg arról, hogy ezek az eljárások megkezdése előtt a következő előfeltételek teljesülése hello:
- Az Azure Data Lake Visual Studio Code eszköz. Útmutatásért lásd: [használata Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).
- C# a Visual Studio Code (Ha azt szeretné, hogy a helyi hibakeresési tooperform egy U-SQL).

   ![Telepítse C# a Data Lake Tools for Visual Studio kód](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-install-ms-vscodecsharp.png)
   
   > [!NOTE]
   > U-SQL helyi futtatása hello és hibakeresési szolgáltatások jelenleg csak Windows felhasználók támogatására. 


## <a name="set-up-hello-u-sql-local-run-environment"></a>Hello U-SQL helyi futtatási környezet beállítása

1. Válassza ki a Ctrl + Shift + P tooopen hello parancs paletta, és írja be **ADL: letöltése LocalRun függőségi** toodownload hello csomagok.  

   ![Hello ADL LocalRun függőségi csomagok letöltése](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/DownloadLocalRun.png)

2. Keresse meg a hello függőségi csomagok hello látható hello elérési útról **kimeneti** ablaktáblán, majd telepítse BuildTools és Win10SDK 10240. Íme egy példa elérési útja:  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency
`  
  ![Keresse meg a hello függőségi csomagok](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)

   a. tooinstall BuildTools, hello varázsló utasításait követve.   

  ![BuildTools telepítése](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   b. tooinstall Win10SDK 10240 hello varázsló utasításait követve.  

  ![Telepítse a Win10SDK 10240](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. Hello környezeti változó beállítása. Set hello **SCOPE_CPP_SDK** környezeti változót:  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency\CppSDK_3rdparty
`  
4. Az operációs rendszer hello toomake meg arról, hogy hello környezetiváltozó-beállításainak érvénybe léptetéséhez indítsa újra.  

   ![Győződjön meg arról, hello SCOPE_CPP_SDK környezeti változó telepítve van](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/ConfigScopeCppSDk.png)

## <a name="start-hello-local-run-service-and-submit-hello-u-sql-job-tooa-local-account"></a>Hello helyi futtatási szolgáltatás elindítása, és küldje el a hello U-SQL projekt tooa helyi fiók 
Hello első felhasználó esetében áll felszólító toodownload hello ADL: letöltése LocalRun függőségi csomagok, ha még nincs telepítve.
1. Válassza ki a Ctrl + Shift + P tooopen hello parancs paletta, és írja be **ADL: helyi futtatása szolgáltatás indítása**.
2. Válassza ki **elfogadás** tooaccept hello Microsoft szoftverlicenc-szerződés hello első alkalommal. 

   ![Hello Microsoft szoftverlicenc-feltételek elfogadása](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. hello cmd konzol megnyitása. A felhasználók első alkalommal kell tooenter **3**, és keresse meg a bemeneti és kimeneti adatok hello helyi mappa elérési útját. Egyéb lehetőségek hello alapértelmezett értékeket is használhat. 

   ![A Data Lake Tools for Visual Studio Code helyi futtatási cmd](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. Válassza ki a Ctrl + Shift + P tooopen hello parancs paletta, adja meg **ADL: feladat elküldése**, majd válassza ki **helyi** toosubmit hello feladat tooyour helyi fiók.

   ![A Data Lake Tools for Visual Studio Code helyi kiválasztása](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. Hello feladat elküldése után megtekintheti a hello küldésének adatokat. tooview hello küldésének részleteinek kiválasztása **jobUrl** a hello **kimeneti** ablak. Küldésének feladatállapot hello hello cmd konzolról is megtekintheti. Adja meg **7** hello cmd konzolon Ha azt szeretné, hogy tooknow több feladat részletei.

   ![A Data Lake Visual Studio Code helyi futtatáskor a kimeneti eszközei](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![Data Lake Tools for Visual Studio Code helyi futtatáskor a cmd állapota](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png) 


## <a name="start-a-local-debug-for-hello-u-sql-job"></a>Egy helyi hibakeresési hello U-SQL-feladat indítása  
Hello első felhasználó esetében áll felszólító toodownload hello ADL: letöltése LocalRun függőségi csomagok, ha még nincs telepítve.
  
1. Válassza ki a Ctrl + Shift + P tooopen hello parancs paletta, és írja be **ADL: helyi futtatása szolgáltatás indítása**. hello cmd konzol megnyitása. Győződjön meg arról, hogy hello **DataRoot** van beállítva.
3. A C# háttérkód állítható be töréspont.
4. Hello parancsprogram-szerkesztő, jelölje ki Ctrl + Shift + P tooopen hello parancskonzolról, és írja be **helyi hibakeresése** toostart a helyi hibakeresési szolgáltatás.

![A Data Lake Tools for Visual Studio Code helyi hibakeresési eredménye](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a>Következő lépések
- Azure Data Lake Tools használatával a Visual Studio Code, lásd: [használata Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).
- A Data Lake Analytics elindított adatainak lekérése, lásd: [oktatóanyag: Ismerkedés az Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).
- A Data Lake Tools for Visual Studio információ: [oktatóanyag: fejlesztése U-SQL-parancsfájlok Data Lake Tools for Visual Studio használatával](data-lake-analytics-data-lake-tools-get-started.md).
- Hello információ szerelvények fejlesztésével: [fejlesztése U-SQL-szerelvények Azure Data Lake Analytics-feladatok](data-lake-analytics-u-sql-develop-assemblies.md).
