---
title: "Azure Data Lake Analytics-feladatok aaaDevelop U-SQL-szerelvények |} Microsoft Docs"
description: "Ismerje meg, és hogyan lehet toodevelop szerelvények toobe használja fel újra a Data Lake Analytics feladatok. "
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/30/2016
ms.author: jejiang
ms.openlocfilehash: 86dd17b25e0967306ed36bb5b7f3178d9409d53d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-u-sql-assemblies-for-azure-data-lake-analytics-jobs"></a>Azure Data Lake Analytics-feladatok U-SQL-szerelvények fejlesztése
Ismerje meg, hogyan tooturn háttérkód szerelvények toobe használja, és fel újra a Data Lake Analytics-feladatok be. 

U-SQL lehetővé teszi a saját egyéni kód könnyen tooadd .net nyelven, például a C#, VB.Net vagy F #. Saját futásidejű toosupport más nyelveken is telepíteni.

hello legegyszerűbb módja toouse egyéni kód toouse hello Data Lake Tools Visual Studio háttérkód képességeinek. További információkért lásd: [oktatóanyag: Data Lake Tools for Visual Studio használatával U-SQL-parancsfájlok fejlesztése](data-lake-analytics-data-lake-tools-get-started.md). Van néhány hátrányai háttérkód használatával:

- hello forráskód minden parancsfájl benyújtására lekérdezi feltöltve.
- háttérkód nem osztható meg más feladatok.

tooaddress ezek hátrányai háttérkód ikonná szerelvényeket, és regisztrálja a hello szerelvények toohello Data Lake Analytics-katalógus.

## <a name="prerequisites"></a>Előfeltételek
* A Visual Studio 2017, a Visual Studio 2015, Visual Studio 2013 4. frissítéssel vagy Visual Studio 2012 és telepített Visual C++
* A Microsoft Azure SDK for .NET 2.5-ös verzió vagy újabb.  Telepítse a webplatform-telepítő hello vagy Visual Studio-telepítő
* Data Lake Analytics-fiók.  Lásd: [Az Azure Data Lake Analytics használatának első lépései az Azure Portallal](data-lake-analytics-get-started-portal.md).
* Lépkedjen végig hello [Ismerkedés az Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md) oktatóanyag.
* Csatlakozás tooAzure.
* Hello forrásadatok feltöltése című [Ismerkedés az Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md). 

## <a name="develop-assemblies-for-u-sql"></a>A U-SQL szerelvények fejlesztése

**toocreate, és küldje el a U-SQL-feladatot:**

1. A hello **fájl** menüben kattintson a **új**, és kattintson a **projekt**.
2. Bontsa ki a **telepített**, **sablonok**, **Azure Data Lake**, **U-SQL(ADLA)**, jelölje be hello **Class Library (az U-SQL Alkalmazás)** sablont, és kattintson **OK**.
3. Írja be a kódot Class1.cs.  hello az alábbiakban egy példát az.

        using Microsoft.Analytics.Interfaces;

        namespace USQLApplication_codebehind
        {
            [SqlUserDefinedProcessor]
            public class MyProcessor : IProcessor
            {
                public override IRow Process(IRow input, IUpdatableRow output)
                {
                    output.Set(0, input.Get<string>(0));
                    output.Set(0, input.Get<string>(0));
                    return output.AsReadOnly();
                }
            }
        }
4. Kattintson a hello **Build** menüben, majd kattintson **megoldás fordítása** toocreate hello DLL-fájljában.

## <a name="register-assemblies"></a>Szerelvények regisztrálása

Lásd: [használata Data Lake Analytics(U-SQL) katalógus](data-lake-analytics-use-u-sql-catalog.md).


## <a name="use-hello-assemblies"></a>Hello szerelvények használata

Lásd: [hello Azure Data Lake Tools használja a Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).

## <a name="see-also"></a>Lásd még:
* [A Data Lake Analytics PowerShell használatának első lépései](data-lake-analytics-get-started-powershell.md)
* [Ismerkedés a Data Lake Analytics használatának hello Azure-portálon](data-lake-analytics-get-started-portal.md)
* [Használja a Data Lake Tools for Visual Studio a U-SQL-alkalmazások fejlesztésével](data-lake-analytics-data-lake-tools-get-started.md)
* [Használja a Data Lake Analytics(U-SQL) katalógus](data-lake-analytics-use-u-sql-catalog.md)
* [A Visual Studio Code hello Azure Data Lake Tools használata](data-lake-analytics-data-lake-tools-for-vscode.md)
