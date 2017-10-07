---
title: "aaaScale U-SQL helyi futtatása, és tesztelje az Azure Data Lake U-SQL SDK-val |} Microsoft Docs"
description: "Ismerje meg, hogy miként toouse Azure Data Lake U-SQL SDK tooscale U-SQL feladatok helyi futtatása és a parancssor és a helyi munkaállomáson alkalmazásprogramozási felületek."
services: data-lake-analytics
documentationcenter: 
author: 
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/01/2017
ms.author: yanacai
ms.openlocfilehash: 2b0a16229789268e333f723ff6fc2c3efdc29905
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-u-sql-local-run-and-test-with-azure-data-lake-u-sql-sdk"></a>Skála U-SQL helyi futtatása és a teszt Azure Data Lake U-SQL-SDK-val

U-SQL parancsfájl fejlesztésekor közös toorun és tesztelhet U-SQL parancsfájl helyi előtt küldje el toocloud. Azure Data Lake biztosít az Azure Data Lake U-SQL SDK hívása ebben a forgatókönyvben a Nuget-csomagot, keresztül, amely könnyedén méretezhető U-SQL helyi futtatásával és a vizsgálat. A U-SQL (folyamatos integrációt) CI rendszer tooautomate hello fordítási és tesztelési tesztelést lehetséges toointegrate egyben.

Ha az Ön számára legfontosabb hogyan toomanually helyi futtatásához, és hibakeresési grafikus felhasználói Felülettel tooling U-SQL parancsfájlt, majd használható Azure Data Lake Tools for Visual Studio, amely. Többet is megtudhat [Itt](data-lake-analytics-data-lake-tools-local-run.md).

## <a name="install-azure-data-lake-u-sql-sdk"></a>Telepítés Azure Data Lake U-SQL-SDK

Azure Data Lake U-SQL SDK hello kaphat [Itt](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) a Nuget.org. És annak használata előtt meg kell toomake, az alábbiak szerint függőségeinek rendelkezik.

### <a name="dependencies"></a>Függőségek

Data Lake U-SQL SDK hello hello függőségek a következő szükséges:

- [A Microsoft .NET-keretrendszer 4.6-os vagy újabb](https://www.microsoft.com/download/details.aspx?id=17851).
- Microsoft Visual C++ 14 és a Windows SDK 10.0.10240.0 vagy újabb (amely más néven CppSDK ebben a cikkben). Két módon tooget CppSDK van:

    - Telepítés [Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou). Hello Program Files mappában – például a C:\Program Files (x86) \Windows Kits\10\ kell egy \Windows Kits\10 mappát. Windows 10-es SDK verzió hello \Windows Kits\10\Lib is találhat. Ha nem látja ezeket a mappákat, telepítse újra a Visual Studio, és lehet, hogy tooselect hello Windows 10-es SDK hello telepítése során. Ha ez a Visual Studio telepítve van, hello U-SQL helyi fordító megkeresi azt automatikusan.

    ![A Data Lake Tools for Visual Studio helyi futtatási Windows 10-es SDK](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-windows-10-sdk.png)

    - Telepítés [a Data Lake Tools for Visual Studio](http://aka.ms/adltoolsvs). Hello előre csomagolt Visual C++ és a Windows SDK fájlokra a C:\Program Files (x86) \Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK találja. Ebben az esetben hello U-SQL helyi fordító nem található hello függőségek automatikusan. Az toospecify hello CppSDK elérési út van szüksége. Hello fájlok tooanother helyre másolja, vagy használja a jelenlegi állapotában.

## <a name="understand-basic-concepts"></a>Alapvető fogalmak megismerése

### <a name="data-root"></a>Adatgyökerében

hello adatok-gyökérmappája "helyi tároló" hello helyi számítási fiókhoz. Egyenértékű toohello Azure Data Lake Store-fiók egy Data Lake Analytics-fiók is. Váltás tooa különböző adatok-gyökérmappája hasonlóan tooa különböző store-fiók vált. Ha azt szeretné, hogy tooaccess hagyományosan megosztott adatokat különböző adatgyökerében mappákkal, a parancsfájlok abszolút elérési utakat kell használnia. Vagy hozzon létre fájlt rendszer szimbolikus hivatkozásokat (például **mklink** NTFS) hello adatgyökerében mappa toopoint toohello a megosztott adatok.

hello adatgyökerében mappa a következőkre használható:

- Helyi metaadatainak, például adatbázisok, táblák, táblaértékű függvények (TVFs) és szerelvényeket tárolják.
- Hello bemeneti és kimeneti elérési utak relatív elérési utak a U-SQL definiált kereséséhez. Relatív elérési utak révén könnyebben toodeploy a U-SQL-projektek tooAzure.

### <a name="file-path-in-u-sql"></a>U-SQL-fájl elérési útja

U-SQL-parancsfájlok használhatja relatív elérési út és a helyi abszolút elérési utat. hello relatív elérési út relatív toohello megadott adatok-gyökérmappa elérési útja. Azt javasoljuk, hogy azt használja "/", hello elérési útját elválasztójel toomake hello kiszolgálóoldali kompatibilis a parancsfájlokat. Az alábbiakban néhány olyan relatív elérési útja és az egyenértékű abszolút elérési utakat. Ezekben a példákban C:\LocalRunDataRoot hello adatok-gyökérmappájába.

|Relatív elérési útja|Abszolút elérési útja|
|-------------|-------------|
|/ABC/DEF/input.csv |C:\LocalRunDataRoot\abc\def\input.csv|
|ABC/DEF/input.csv  |C:\LocalRunDataRoot\abc\def\input.csv|
|D:/ABC/DEF/input.csv |D:\abc\def\input.csv|

### <a name="working-directory"></a>Munkakönyvtár

Hello U-SQL parancsfájl helyben fut, amikor egy működő könyvtárba aktuális futtatási könyvtárának a fordítás során jön létre. Ezenkívül toohello fordítási kimenete, hello szükséges futásidejű fájlok helyi végrehajtásra lesznek árnyékmásolat másolt toothis munkakönyvtár. directory legfelső szintű munkamappa hello "ScopeWorkDir" nevezik, és a munkakönyvtár hello hello fájlok a következők:

|Könyvtár vagy fájl|Könyvtár vagy fájl|Könyvtár vagy fájl|Meghatározás|Leírás|
|--------------|--------------|--------------|----------|-----------|
|C6A101DDCB470506| | |Kivonat karakterláncot futtatókörnyezet-verzió|A helyi végrehajtásához szükséges futásidejű fájlok árnyékmásolatát|
| |Script_66AE4909AA0ED06C| |Parancsfájl neve + kivonat karakterláncot a parancsprogram elérési útja|Fordítási kimenetek és végrehajtási lépés a naplózás|
| | |\_parancsfájl\_.abr|A fordítóprogram kimenetének|Algebra fájl|
| | |\_ScopeCodeGen\_. *|A fordítóprogram kimenetének|A felügyelt kód jön létre|
| | |\_ScopeCodeGenEngine\_. *|A fordítóprogram kimenetének|Létrehozott natív kód|
| | |hivatkozott szerelvények|Szerelvényre mutató hivatkozás|Hivatkozott szerelvény fájlok|
| | |deployed_resources|Erőforrások telepítése|Erőforrás-telepítési fájlok|
| | |xxxxxxxx.xxx[1..n]\_\*. *|Végrehajtási napló|A végrehajtási lépések napló|


## <a name="use-hello-sdk-from-hello-command-line"></a>Hello parancssorból SDK hello használata

### <a name="command-line-interface-of-hello-helper-application"></a>Parancssori felületének hello segédalkalmazással.

Az SDK directory\build\runtime LocalRunHelper.exe hello parancssori segítő alkalmazással, amely felületek helyi futtatási általánosan használt hello toomost funkciókat biztosít. Ne feledje, hogy mindkét hello parancs hello argumentum kapcsolók a következők kis-és nagybetűket. tooinvoke azt:

    LocalRunHelper.exe <command> <Required-Command-Arguments> [Optional-Command-Arguments]

Paraméterek nélkül, vagy a hello fusson a LocalRunHelper.exe **súgó** tooshow hello súgóinformációval váltani:

    > LocalRunHelper.exe help

        Command 'help' :  Show usage information
        Command 'compile' :  Compile hello script
        Required Arguments :
            -Script param
                    Script File Path
        Optional Arguments :
            -Shallow [default value 'False']
                    Shallow compile

A hello segítő információk:

-  **A parancs** által biztosított hello parancs nevét.  
-  **Kötelező argumentum** , meg kell adni argumentumait sorolja fel.  
-  **Nem kötelező argumentumában** sorolja fel, ez nem kötelező megadni, alapértelmezett értékekkel.  Nem kötelező, logikai típusú argumentumokat nem kell a paramétereket, és azok megjelenésének negatív tootheir alapértelmezett értéket jelenti.

### <a name="return-value-and-logging"></a>Visszatérési érték és a naplózás

hello segítő alkalmazással adja vissza **0** a sikeres és **-1** sikertelen. Alapértelmezés szerint a hello segítő küld minden üzenetek toohello jelenlegi konzolt. Azonban a legtöbb hello parancs támogatja hello **- MessageOut naplófájl_elérési_útja** nem kötelező argumentum, amely átirányítja a hello kimenete tooa naplófájlt.

### <a name="environment-variable-configuring"></a>Környezeti változó beállítása

U-SQL helyi futnia kell a megadott adatok legfelső szintű helyi storage-fiók, valamint egy adott CppSDK függőségek elérési úton. Mindkét hello argumentum parancssori vagy beállított környezeti változóban végezhető el őket.

- Set hello **SCOPE_CPP_SDK** környezeti változó.

    Ha a Microsoft Visual C++ és hello Windows SDK-t úgy, hogy a Data Lake Tools for Visual Studio telepítése, győződjön meg arról, hogy rendelkezik-e a következő mappa hello:

        C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK

    Adja meg egy új környezeti változó neve **SCOPE_CPP_SDK** toopoint toothis könyvtár. Vagy másolja hello mappa toohello más helyre, és adja meg **SCOPE_CPP_SDK** , mint a.

    Továbbá toosetting hello környezeti változóban, megadhatja a hello **- CppSDK** használatakor hello parancssori argumentum. Ennek az argumentumnak felülírja az alapértelmezett CppSDK környezeti változó.

- Set hello **LOCALRUN_DATAROOT** környezeti változó.

    Adja meg egy új környezeti változó neve **LOCALRUN_DATAROOT** toohello adatgyökerében mutat.

    Továbbá toosetting hello környezeti változóban, megadhatja a hello **- DataRoot** hello adatgyökerében elérési útja, ha a parancssor használata argumentumot. Ennek az argumentumnak felülírja az alapértelmezett adatgyökerében környezeti változó. Tooadd kell ez argumentum tooevery a parancssor futtatja, hogy felülírhatja hello alapértelmezett adatgyökerében környezeti változó minden műveletnél.

### <a name="sdk-command-line-usage-samples"></a>SDK parancssori használati minták

#### <a name="compile-and-run"></a>Fordítás és futtatás

Hello **futtatása** parancsnak toocompile hello parancsfájlt, és hajthat végre a lefordított eredmények. Parancssori argumentumok szerintiek kombinációja **fordítási** és **hajtható végre**.

    LocalRunHelper run -Script path_to_usql_script.usql [optional_arguments]

hello a következők nem kötelező argumentumainak **futtatása**:


|Argumentum|Alapértelmezett érték|Leírás|
|--------|-------------|-----------|
|-Háttérkódban|False (Hamis)|hello parancsfájl .cs kód tartozik.|
|-CppSDK| |CppSDK könyvtár|
|-DataRoot| DataRoot környezeti változó|A helyi futtató DataRoot alapértelmezett túl "LOCALRUN_DATAROOT" környezeti változó|
|-MessageOut| |A konzol tooa fájl memóriakép üzenetek|
|-Párhuzamos|1|Futtassa a hello hello terv megadott párhuzamossági|
|-Hivatkozások| |Elérési utak tooextra referenciaszerelvények vagy listája adatforrás adatfájljainak tartozó kódot, szóközzel elválasztva ';'|
|-UdoRedirect|False (Hamis)|Udo szerelvény átirányítási config készítése|
|-UseDatabase|master|Az ideiglenes szerelvények regisztrálásakor háttérkódot adatbázis toouse|
|-Verbose|False (Hamis)|A futásidejű részletes kimenetének megjelenítése|
|-WorkDir|Aktuális könyvtárhoz|A fordító használati és a kimeneti könyvtár|
|-RunScopeCEP|0|ScopeCEP mód toouse|
|-ScopeCEPTempPath|TEMP|A streamelési adatok ideiglenes elérési toouse|
|-OptFlags| |Optimalizáló jelzők vesszővel elválasztott listája|


Íme egy példa:

    LocalRunHelper run -Script d:\test\test1.usql -WorkDir d:\test\bin -CodeBehind -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB –Parallel 5 -Verbose

Egyidejű mellett **fordítási** és **hajtható végre**, fordítása, és külön-külön végrehajtása fordítása hello végrehajtható fájlokat.

#### <a name="compile-a-u-sql-script"></a>A U-SQL parancsfájl összeállítása

Hello **fordítási** parancs használt toocompile egy U-SQL parancsfájl tooexecutables.

    LocalRunHelper compile -Script path_to_usql_script.usql [optional_arguments]

hello a következők nem kötelező argumentumainak **fordítási**:


|Argumentum|Leírás|
|--------|-----------|
| -Háttérkódban [alapértelmezett értéke "False"]|hello parancsfájl .cs kód tartozik.|
| -CppSDK [alapértelmezett érték "]|CppSDK könyvtár|
| -DataRoot [alapértelmezett érték "DataRoot környezeti változó"]|A helyi futtató DataRoot alapértelmezett túl "LOCALRUN_DATAROOT" környezeti változó|
| -MessageOut [alapértelmezett érték "]|A konzol tooa fájl memóriakép üzenetek|
| -Hivatkozik [alapértelmezett érték "]|Elérési utak tooextra referenciaszerelvények vagy listája adatforrás adatfájljainak tartozó kódot, szóközzel elválasztva ';'|
| -Sekély [alapértelmezett értéke "False"]|Sekély fordítási|
| -UdoRedirect [alapértelmezett értéke "False"]|Udo szerelvény átirányítási config készítése|
| -UseDatabase [alapértelmezett érték "master"]|Az ideiglenes szerelvények regisztrálásakor háttérkódot adatbázis toouse|
| -WorkDir [alapértelmezett érték "Az aktuális könyvtár"]|A fordító használati és a kimeneti könyvtár|
| -RunScopeCEP [alapértelmezett érték "0"]|ScopeCEP mód toouse|
| -ScopeCEPTempPath [alapértelmezett érték "temp"]|A streamelési adatok ideiglenes elérési toouse|
| -OptFlags [alapértelmezett érték "]|Optimalizáló jelzők vesszővel elválasztott listája|


Az alábbiakban néhány használati példák.

A U-SQL parancsfájl összeállítása:

    LocalRunHelper compile -Script d:\test\test1.usql

A U-SQL parancsfájl összeállításához, és állítsa be a hello adatok-gyökérmappájába. Vegye figyelembe, hogy ezzel a művelettel felülírom hello beállítása környezeti változó.

    LocalRunHelper compile -Script d:\test\test1.usql –DataRoot c:\DataRoot

U-SQL parancsfájl fordítása, és egy működő könyvtárba, a referencia szerelvény és az adatbázis beállítása:

    LocalRunHelper compile -Script d:\test\test1.usql -WorkDir d:\test\bin -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB

#### <a name="execute-compiled-results"></a>Lefordított eredmények végrehajtása

Hello **hajtható végre** parancs lefordítva használt tooexecute eredmények.   

    LocalRunHelper execute -Algebra path_to_compiled_algebra_file [optional_arguments]

hello a következők nem kötelező argumentumainak **hajtható végre**:

|Argumentum|Leírás|
|--------|-----------|
|-DataRoot [alapértelmezett érték "]|A metaadatok végrehajtás adatgyökerében. Alapértelmezés szerint toohello **LOCALRUN_DATAROOT** környezeti változó.|
|-MessageOut [alapértelmezett érték "]|Memóriakép hello tooa fájlban üzeneteket.|
|-Párhuzamos [alapértelmezett értéke "1"]|Kijelző toorun hello létrehozott helyi futtatási lépéseket hello megadott párhuzamossági szintjét.|
|-Verbose [alapértelmezett érték "Hamis"]|Kijelző tooshow futásidejű kimeneteinek részletes.|

Íme egy példa:

    LocalRunHelper execute -Algebra d:\test\workdir\C6A101DDCB470506\Script_66AE4909AA0ED06C\__script__.abr –DataRoot c:\DataRoot –Parallel 5


## <a name="use-hello-sdk-with-programming-interfaces"></a>Alkalmazásprogramozási felületek hello SDK használata

hello alkalmazásprogramozási felületek a hello LocalRunHelper.exe találhatók. U-SQL SDK hello toointegrate hello funkcióit használhat, és C# teszt keretében tooscale hello a U-SQL parancsfájl helyi tesztelése. Ebben a cikkben használom hello szabványos C# egység teszt projekt tooshow hogyan toouse ezek felületeihez tootest a U-SQL parancsfájlt.

### <a name="step-1-create-c-unit-test-project-and-configuration"></a>1. lépés: A C# egység teszt projekt és konfigurációs létrehozása

- Hozzon létre egy C# egység teszt projektet fájl > Új > Projekt > Visual C# > tesztelése > egység tesztelése projekt.
- Adja hozzá a LocalRunHelper.exe hello projekt hivatkozásként. hello LocalRunHelper.exe itt található: \build\runtime\LocalRunHelper.exe a Nuget-csomagot.

    ![Azure Data Lake U-SQL SDK hivatkozás hozzáadása](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-add-reference.png)

- U-SQL SDK **csak** támogatási x64 környezetben, győződjön meg arról, hogy tooset build platform cél x64 szerint. Beállíthatja, hogy a projekt tulajdonságon keresztül > Build > Platform cél.

    ![Azure Data Lake U-SQL SDK konfigurálása x64 projekt](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-x64.png)

- Győződjön meg arról, hogy tooset tesztelési környezetben, x64. A Visual Studio állíthatja keresztül Test > vizsgálati beállítások > alapértelmezett processzorarchitektúra > x64.

    ![Azure Data Lake U-SQL SDK konfigurálása x64 tesztelési környezetben](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-test-x64.png)

- Győződjön meg arról, hogy toocopy NugetPackage\build\runtime\ tooproject munkakönyvtár általában ProjectFolder\bin\x64\Debug alatt álló összes függőség fájlra.

### <a name="step-2-create-u-sql-script-test-case"></a>2. lépés: A U-SQL parancsfájl Teszteset létrehozása

Az alábbiakban van hello mintakód a U-SQL parancsfájl teszteléséhez. Tesztelési, tooprepare parancsfájlok kell bemeneti fájlok és a várt kimeneti fájlokat.

    using System;
    using Microsoft.VisualStudio.TestTools.UnitTesting;
    using System.IO;
    using System.Text;
    using System.Security.Cryptography;
    using Microsoft.Analytics.LocalRun;

    namespace UnitTestProject1
    {
        [TestClass]
        public class USQLUnitTest
        {
            [TestMethod]
            public void TestUSQLScript()
            {
                //Specify hello local run message output path
                StreamWriter MessageOutput = new StreamWriter("../../../log.txt");

                LocalRunHelper localrun = new LocalRunHelper(MessageOutput);

                //Configure hello DateRoot path, Script Path and CPPSDK path
                localrun.DataRoot = "../../../";
                localrun.ScriptPath = "../../../Script/Script.usql";
                localrun.CppSdkDir = "../../../CppSDK";

                //Run U-SQL script
                localrun.DoRun();

                //Script output 
                string Result = Path.Combine(localrun.DataRoot, "Output/result.csv");

                //Expected script output
                string ExpectedResult = "../../../ExpectedOutput/result.csv";

                Test.Helpers.FileAssert.AreEqual(Result, ExpectedResult);

                //Don't forget tooclose MessageOutput tooget logs into file
                MessageOutput.Close();
            }
        }
    }

    namespace Test.Helpers
    {
        public static class FileAssert
        {
            static string GetFileHash(string filename)
            {
                Assert.IsTrue(File.Exists(filename));

                using (var hash = new SHA1Managed())
                {
                    var clearBytes = File.ReadAllBytes(filename);
                    var hashedBytes = hash.ComputeHash(clearBytes);
                    return ConvertBytesToHex(hashedBytes);
                }
            }

            static string ConvertBytesToHex(byte[] bytes)
            {
                var sb = new StringBuilder();

                for (var i = 0; i < bytes.Length; i++)
                {
                    sb.Append(bytes[i].ToString("x"));
                }
                return sb.ToString();
            }

            public static void AreEqual(string filename1, string filename2)
            {
                string hash1 = GetFileHash(filename1);
                string hash2 = GetFileHash(filename2);

                Assert.AreEqual(hash1, hash2);
            }
        }
    }


### <a name="programming-interfaces-in-localrunhelperexe"></a>A LocalRunHelper.exe felületek

LocalRunHelper.exe biztosít programozási felületek U-SQL helyi fordítási, futtassa a hello, stb. hello felületek az alábbiak szerint vannak felsorolva.

**Konstruktor**

nyilvános LocalRunHelper ([System.IO.TextWriter messageOutput = null])

|Paraméter|Típus|Leírás|
|---------|----|-----------|
|messageOutput|System.IO.TextWriter|a kimeneti üzenetek be toonull toouse konzol|

**Tulajdonságok**

|Tulajdonság|Típus|Leírás|
|--------|----|-----------|
|AlgebraPath|Karakterlánc|hello elérési tooalgebra fájl (algebra fájl egyike hello fordítási eredmények)|
|CodeBehindReferences|Karakterlánc|Ha hello parancsfájl további háttérkódot hivatkozásokat, adja meg a hello elérési útjait határolt ";"|
|CppSdkDir|Karakterlánc|CppSDK könyvtár|
|CurrentDir|Karakterlánc|Aktuális könyvtárhoz|
|DataRoot|Karakterlánc|Adatok elérési útjának gyökeréhez|
|DebuggerMailPath|Karakterlánc|hello elérési toodebugger mailslot|
|GenerateUdoRedirect|logikai érték|Ha azt szeretné, hogy toogenerate szerelvény betöltése a átirányítási config felülbírálása|
|HasCodeBehind|logikai érték|Ha hello parancsfájl kód tartozik.|
|InputDir|Karakterlánc|A bemeneti adatok könyvtár|
|MessagePath|Karakterlánc|Üzenet memóriakép fájl elérési útja|
|OutputDir|Karakterlánc|A kimeneti adatok könyvtár|
|Párhuzamos végrehajtás|int|Párhuzamossági toorun hello algebra|
|ParentPid|int|A mely hello szolgáltatás figyeli tooexit, set too0 vagy negatív tooignore hello szülő azonosítója (PID)|
|ResultPath|Karakterlánc|Eredmény memóriakép fájl elérési útja|
|RuntimeDir|Karakterlánc|Futásidejű könyvtár|
|scriptPath|Karakterlánc|Ha toofind hello parancsfájl|
|Sekély|logikai érték|Vagy nem a fordítási sekély|
|TempDir|Karakterlánc|Ideiglenes könyvtár|
|UseDataBase|Karakterlánc|Adja meg az ideiglenes szerelvények regisztrálásakor, alapértelmezés szerint fő háttérkódot hello adatbázis toouse|
|WorkDir|Karakterlánc|Előnyben részesített munkakönyvtár|


**Módszer**

|Módszer|Leírás|térjen vissza|Paraméter|
|------|-----------|------|---------|
|nyilvános bool DoCompile()|Hello U-SQL parancsfájl összeállítása|Sikeres művelet igaz| |
|nyilvános bool DoExec()|Végrehajtása fordítása hello eredménye|Sikeres művelet igaz| |
|nyilvános bool DoRun()|(Fordítási + Execute) hello U-SQL-parancsfájl futtatása|Sikeres művelet igaz| |
|nyilvános bool IsValidRuntimeDir (karakterlánc elérési út)|Ellenőrizze, hogy a megadott elérési út hello érvényes futásidejű elérési útja|Igaz a érvényes|hello futásidejű könyvtár elérési útja|


## <a name="faq-about-common-issue"></a>Közös problémával kapcsolatos gyakori kérdések

### <a name="error-1"></a>1. hiba:
E_CSC_SYSTEM_INTERNAL: Belső hiba! Nem sikerült betölteni a fájl vagy szerelvény "ScopeEngineManaged.dll" vagy annak valamelyik függősége. hello megadott modul nem található.

Ellenőrizze a következőket hello:

- Győződjön meg arról, hogy x64 környezetben. hello build célplatform és hello tesztkörnyezetben kell x64, tekintse meg a túl**1. lépés: egység létrehozása a C# projekt és konfigurációs teszteléséhez** felett.
- Ellenőrizze, hogy az összes függőség fájlra NugetPackage\build\runtime\ tooproject munkakönyvtár másolta.


## <a name="next-steps"></a>Következő lépések

* toolearn U-SQL, lásd: [Ismerkedés az Azure Data Lake Analytics U-SQL nyelv](data-lake-analytics-u-sql-get-started.md).
* toolog diagnosztikai információkat, lásd: [diagnosztikai naplók elérése az Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).
* egy összetettebb lekérdezés toosee lásd [Azure Data Lake Analytics használatával webhelyek naplóinak elemzése](data-lake-analytics-analyze-weblogs.md).
* feladat részletei tooview, lásd: [használata feladat böngésző és az Azure Data Lake Analytics-feladatok feladat megtekintése](data-lake-analytics-data-lake-tools-view-jobs.md).
* toouse hello vertex végrehajtási nézetet, lásd: [használata hello Vertex végrehajtási nézetet a Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
