---
title: "aaaPersist feladat- és kimeneti tooAzure tárolás az Azure Batch szolgáltatás API hello |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Batch szolgáltatás API toopersist kötegelt feladat és a feladat kimenetét tooAzure tároló."
services: batch
author: tamram
manager: timlt
editor: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: tamram
ms.openlocfilehash: 71b3f7c0dda2d2a9d8eb3eef83229873c70ca22c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="persist-task-data-tooazure-storage-with-hello-batch-service-api"></a>Feladat adatok tooAzure tárolási hello Batch szolgáltatás API megőrzése

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Verziójával 2017-05-01-től kezdődően hello Batch szolgáltatás API támogatja tárolásakor kimeneti adatok tooAzure tároló és a készletek hello virtuálisgép-konfiguráció futó feladat manager feladatok. Ha hozzáad egy feladatot, hello feladat kimenetének hello célját az Azure Storage megadhatja egy tárolót. hello Batch szolgáltatás ezután ír a kimeneti adatok toothat tároló, hello feladat befejezésekor.

Egy előny toousing hello Batch szolgáltatás API toopersist feladat kimenetének, ezért nincs szükség a feladat hello toomodify hello alkalmazás fut. Ehelyett néhány egyszerű módosítások tooyour ügyfélalkalmazást, az hello kód által létrehozott hello tevékenység kimenete hello feladat is megmaradnak.   

## <a name="when-do-i-use-hello-batch-service-api-toopersist-task-output"></a>Ha használja a Batch szolgáltatás API hello toopersist feladat kimenetének?

Az Azure Batch szolgáltatás több mint egyirányú toopersist feladat kimenete. Hello Batch szolgáltatás API használata egy kényelmes módszert alkalmaz, amely a legjobb olyan környezethez a legalkalmasabb toothese forgatókönyvek van:

- Azt szeretné, toowrite kód toopersist tevékenység kimenete az ügyfélalkalmazásban hello alkalmazás, amely a feladat futásának módosítása nélkül.
- Azt szeretné, hogy a kötegelt és feladat manager feladatok hello virtuálisgép-konfiguráció létre készletek toopersist kimenetét.
- Azt szeretné, hogy toopersist kimeneti tooan Azure Storage-tárolóban egy tetszőleges nevet.
- Azt szeretné, hogy toopersist kimeneti tooan Azure Storage nevű tárolót függően toohello [kötegelt fájl egyezmények standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). 

Ha a forgatókönyv eltér a fent felsorolt, szükség lehet tooconsider másik módszert. Például hello Batch szolgáltatás API jelenleg nem támogatja az adatfolyam kimeneti tooAzure tárolási hello feladat futása közben. kimeneti toostream, érdemes lehet hello kötegelt fájl egyezmények szalagtár használata esetén elérhető, a .NET-hez. A többi nyelvet kell tooimplement saját megoldás. Más beállításokat a tárolásakor feladat kimenetének további információkért lásd: [Persist feladat- és kimeneti tooAzure tárolási](batch-task-output.md). 

## <a name="create-a-container-in-azure-storage"></a>A tároló létrehozása az Azure Storage

toopersist tevékenység kimeneti tooAzure tárolási, meg kell toocreate olyan tároló, amely a kimeneti fájlok hello célhelyként szolgál. Hello tároló létrehozása, a feladat a feladat mentése előtt minél futtatása előtt. toocreate hello tároló, használjon hello megfelelő Azure Storage ügyféloldali kódtára vagy SDK-val. Azure Storage API-k kapcsolatos további információkért lásd: hello [Azure Storage-dokumentációt](https://docs.microsoft.com/azure/storage/).

Például, ha az alkalmazás írása C# nyelven íródtak, használja a hello [Azure Storage ügyféloldali kódtára a .NET](https://www.nuget.org/packages/WindowsAzure.Storage/). a következő példa azt mutatja meg hogyan hello toocreate egy tárolót:

```csharp
CloudBlobContainer container = storageAccount.CreateCloudBlobClient().GetContainerReference(containerName);
await conainer.CreateIfNotExists();
```

## <a name="get-a-shared-access-signature-for-hello-container"></a>A közös hozzáférésű jogosultságkód hello tároló beolvasása

Hello tároló létrehozása után beolvasása a közös hozzáférésű jogosultságkód (SAS) írási toohello tárolóval. SAS-kód toohello tároló delegált hozzáférést biztosít. hello SAS hozzáférést biztosít az egy megadott készlet azon engedélyek és a megadott időtartam alatt. hello Batch szolgáltatás tárolóval írási engedélyek toowrite tevékenység kimeneti toohello SAS kell. További információ a SAS: [használata közös hozzáférésű jogosultságkód \(SAS\) az Azure Storage](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

Amikor egy SAS hello Azure Storage API-k használata, hello API SAS token karakterláncot ad vissza. A lexikális elem karakterlánca hello SAS, beleértve a hello engedélyek és hello keresztül mely hello SAS érvényességi időtartama az összes paramétereket tartalmaz. toouse hello SAS tooaccess az Azure Storage tárolója, meg kell tooappend hello SAS-token karakterlánc toohello erőforrás URI azonosítója. hello erőforrás URI, együtt hello hozzáfűzi a SAS-jogkivonat, hitelesített hozzáférést tooAzure tárolási biztosít.

hello alábbi példa bemutatja, hogyan a csak írható SAS tooget token-karakterlánc hello tároló, majd hozzáfűzi hello SAS toohello tároló URI:

```csharp
string containerSasToken = container.GetSharedAccessSignature(new SharedAccessBlobPolicy()
{
    SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddDays(1),
    Permissions = SharedAccessBlobPermissions.Write
});

string containerSasUrl = container.Uri.AbsoluteUri + containerSasToken; 
```

## <a name="specify-output-files-for-task-output"></a>Adja meg a feladat kimenete a kimeneti fájlok

egy feladatot, kimeneti fájlok toospecify, hozzon létre egy gyűjteményt a [eredményfájl](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfile) objektumokat, és rendelje hozzá toohello [CloudTask.OutputFiles](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudtask.outputfiles#Microsoft_Azure_Batch_CloudTask_OutputFiles) tulajdonság hello feladat létrehozásakor. 

hello alábbi .NET példakód feladatot hoz létre, amely véletlenszerű számok tooa fájlt ír `output.txt`. hello parancs létrehozza a kimeneti fájl `output.txt` toobe toohello tároló írása. hello példa is hoz létre a kimeneti fájlok bármely naplófájlok hello fájlminta megfelelő `std*.txt` (_pl._, `stdout.txt` és `stderr.txt`). hello létrehozott SAS hello tároló URL-címet korábban hello tároló van szükség. hello Batch szolgáltatás hello SAS tooauthenticate hozzáférés toohello tárolót használja: 

```csharp
new CloudTask(taskId, "cmd /v:ON /c \"echo off && set && (FOR /L %i IN (1,1,100000) DO (ECHO !RANDOM!)) > output.txt\"")
{
    OutputFiles = new List<OutputFile>
    {
        new OutputFile(
            filePattern: @"..\std*.txt",
            destination: new OutputFileDestination(
         new OutputFileBlobContainerDestination(
                    containerUrl: containerSasUrl,
                    path: taskId)),
            uploadOptions: new OutputFileUploadOptions(
            uploadCondition: OutputFileUploadCondition.TaskCompletion)),
        new OutputFile(
            filePattern: @"output.txt",
            destination: 
         new OutputFileDestination(new OutputFileBlobContainerDestination(
                    containerUrl: containerSasUrl,
                    path: taskId + @"\output.txt")),
            uploadOptions: new OutputFileUploadOptions(
            uploadCondition: OutputFileUploadCondition.TaskCompletion)),
}
```

### <a name="specify-a-file-pattern-for-matching"></a>Az egyező fájl minta

Kimeneti fájl megadása esetén használható hello [OutputFile.FilePattern](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfile.filepattern#Microsoft_Azure_Batch_OutputFile_FilePattern) tulajdonság toospecify egy fájl mintát egyeztetéséhez. hello fájlminta előfordulhat, hogy felel meg, nulla fájlok, egyetlen fájl vagy a hello feladat által létrehozott fájlokat.

Hello **FilePattern** tulajdonság támogatja a szabványos fájlrendszere helyettesítő karaktereket, mint `*` (a nem rekurzív megegyezik) és `**` (a rekurzív megegyezik). Például adja meg a fenti hello kódminta hello fájl mintát toomatch `std*.txt` nem rekurzív módon: 

`filePattern: @"..\std*.txt"`

tooupload egyetlen fájl, a fájl minta nem használható helyettesítő adható meg. Például adja meg a fenti hello kódminta hello fájl mintát toomatch `output.txt`:

`filePattern: @"output.txt"`

### <a name="specify-an-upload-condition"></a>Adjon meg egy feltöltési állapot

Hello [OutputFileUploadOptions.UploadCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileuploadoptions.uploadcondition#Microsoft_Azure_Batch_OutputFileUploadOptions_UploadCondition) tulajdonság lehetővé teszi a kimeneti fájlok feltételes feltöltése. Egy általános forgatókönyv tooupload egy fájlt, ha hello feladat sikeres, és egy másik készlet fájlok akkor, ha nem sikerül. Előfordulhat például tooupload részletes naplófájlok azt szeretné, csak akkor, ha hello feladat sikertelen lesz, és egy nem nulla kilépési kód kilép. Hasonlóképpen érdemes lehet tooupload eredmény fájlok csak hello feladat sikeres, ha azokat a fájlokat esetleg hiányzik vagy nem teljes, ha hello feladat sikertelen.

hello fenti beállítja hello **UploadCondition** tulajdonság túl**TaskCompletion**. A beállítással megadható, hogy hello fájl feltöltése után hello feladatok, függetlenül hello kilépési kód hello értékének toobe. 

`uploadCondition: OutputFileUploadCondition.TaskCompletion`

Egyéb beállításokat, lásd: hello [OutputFileUploadCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.common.outputfileuploadcondition) felsorolás.

### <a name="disambiguate-files-with-hello-same-name"></a>Elem egyértelműségének biztosításához hello fájlok ugyanazzal a névvel

hello feladatok feladatokban hello rendelkező fájlokat is eredményezhet, ugyanazzal a névvel. Például `stdout.txt` és `stderr.txt` feladatokban futó minden feladatot hoz létre. Mivel minden tevékenység saját környezetében fut, ezek a fájlok hello csomópont fájlrendszeren nem ütköznek. Azonban a megosztott tároló több feladatok tooa fájlok feltöltésekor lesz szüksége hello toodisambiguate fájlok ugyanazzal a névvel.

Hello [OutputFileBlobContainerDestination.Path](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileblobcontainerdestination.path#Microsoft_Azure_Batch_OutputFileBlobContainerDestination_Path) tulajdonság határozza meg a cél blob hello vagy virtuális könyvtárat a kimeneti fájlokat. Használhatja a hello **elérési** tulajdonság tooname hello blob vagy virtuális könyvtárat úgy, hogy a kimeneti hello az Azure Storage egyedileg megnevezett ugyanazzal a névvel rendelkező fájlok. Hello feladat azonosítójával hello elérési út egy jó módszer tooensure egyedi nevek és könnyebb a fájlok azonosításához.

Ha hello **FilePattern** tulajdonsága tooa helyettesítő karakteres kifejezést, majd az összes, amely megfelel a mintának hello fájljai feltöltött toohello hello által megadott virtuális könyvtár **elérési** tulajdonság. Például ha hello tároló van `mycontainer`, hello tevékenység azonosítója `mytask`, és hello fájl minta `..\std*.txt`, majd hello abszolút URI-azonosítók toohello kimeneti fájlok az Azure Storage hasonló lesz:

```
https://myaccount.blob.core.windows.net/mycontainer/mytask/stderr.txt
https://myaccount.blob.core.windows.net/mycontainer/mytask/stdout.txt
```

Ha hello **FilePattern** tulajdonság set toomatch egyetlen fájlnév, ami azt jelenti, nem tartalmaz helyettesítő karaktereket, majd hello hello értékének **elérési** tulajdonság határozza meg a hello teljesen minősített blob neve . Ha várhatóan több feladat egyetlen fájl ütközik elnevezési, szerepeltesse hello virtuális könyvtár nevét hello hello fájl neve toodisambiguate részeként azokat a fájlokat. Például set hello **elérési** tulajdonság tooinclude hello tevékenység azonosítója, hello elválasztó karaktert (általában egy perjel) és hello fájlnév:

`path: taskId + @"/output.txt"`

hello abszolút URI-azonosítók toohello kimeneti fájlok feladatok számú hasonló lesz:

```
https://myaccount.blob.core.windows.net/mycontainer/task1/output.txt
https://myaccount.blob.core.windows.net/mycontainer/task2/output.txt
```

További információ a virtuális könyvtárak az Azure Storage: [hello a tárolóban lévő blobok listázása](../storage/blobs/storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container).


## <a name="diagnose-file-upload-errors"></a>Fájl feltöltése hibák

Ha feltöltése a kimeneti fájlok tooAzure tárolási sikertelen, akkor hello feladat áthelyezi toohello **befejezve** állapotát és hello [TaskExecutionInformation.FailureInformation](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskexecutioninformation.failureinformation#Microsoft_Azure_Batch_TaskExecutionInformation_FailureInformation) tulajdonság értéke. Vizsgálja meg a hello **FailureInformation** tulajdonság toodetermine milyen hiba történt. Íme például, ha hello tároló nem található a fájl feltöltése előforduló hiba: 

```
Category: UserError
Code: FileUploadContainerNotFound
Message: One of hello specified Azure container(s) was not found while attempting tooupload an output file
```

Minden fájl feltöltése a Batch ír két naplófájl fájlok toohello számítási csomópont `fileuploadout.txt` és `fileuploaderr.txt`. A napló fájlok toolearn egy adott hibával kapcsolatos további ellenőrizheti. Azokban az esetekben, ahol hello fájl feltöltése soha nem kíséreltek meg, például mert maga hello feladat futtatása nem sikerült majd a naplófájlok nem létezik.

## <a name="diagnose-file-upload-performance"></a>Diagnosztizálhatja fájl feltöltése

Hello `fileuploadout.txt` fájl feltöltési folyamatáról naplózza. Láthatja, hogy a fájl toolearn további információk a fájlfeltöltések mennyi ideig tart. Ne feledje, hogy nincsenek sok tényezőkről tooupload teljesítmény, beleértve hello csomópont, más tevékenység hello csomóponton hello feltöltés, a hello időpontjában hello méretét, hogy hello céltároló van hello hello Batch-készlet és ugyanabban a régióban, hogy hány csomópontra van Feltöltés toohello tárfiók: hello azonos időben, és így tovább.

## <a name="use-hello-batch-service-api-with-hello-batch-file-conventions-standard"></a>Hello kötegelt fájl egyezmények szabványos hello Batch szolgáltatás API használata

Megőrizni a feladat kimenete a Batch szolgáltatás API hello, amikor a célként megadott tároló nevére, és blobok azonban van lehetősége. Másik lehetőségként tooname őket toohello szerint [kötegelt fájl egyezmények standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). hello fájl egyezmények standard hello céltárolója és a megadott kimeneti fájl alapján hello nevei hello feladat- és az Azure Storage blob hello nevét határozza meg. Ha hello fájl egyezmények szabványos kimeneti fájlok elnevezési, akkor a kimeneti fájlok megtekintését a hello [Azure-portálon](https://portal.azure.com).

Fejlesztői C# nyelven íródtak, módszerekkel hello hello épített [kötegelt fájl egyezmények .NET-keretrendszerhez készült](https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files). Ezt a szalagtárat a tárolók és a blob-elérési útnak megfelelően nevű meg hello hoz létre. Például hívása hello API tooget hello megfelelő hello tároló nevét, a hello feladatnév alapján:

```csharp
string containerName = job.OutputStorageContainerName();
```

Használhatja a hello [CloudJobExtensions.GetOutputStorageContainerUrl](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files.cloudjobextensions.getoutputstoragecontainerurl) metódus tooreturn közös hozzáférésű jogosultságkód (SAS) URL-Címeket használt toowrite toohello tároló. Ezután adja át a biztonsági Társítások toohello [OutputFileBlobContainerDestination](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileblobcontainerdestination) konstruktor.

Ha C# nyelven fejleszt, szüksége lesz tooimplement hello fájl egyezmények standard saját maga.

## <a name="code-sample"></a>Kódminta

Hello [PersistOutputs] [ github_persistoutputs] mintaprojektet egyike hello [Azure Batch-Kódminták] [ github_samples] a Githubon. A Visual Studio megoldás bemutatja, hogyan toouse hello kötegelt ügyféloldali kódtára a .NET toopersist tevékenység kimeneti toodurable tároló. toorun hello mintát, kövesse az alábbi lépéseket:

1. Nyissa meg hello projektre a **Visual Studio 2015-ös vagy újabb**.
2. A kötegelt és a tárolási **fiók hitelesítő adatait** túl**AccountSettings.settings** hello Microsoft.Azure.Batch.Samples.Common projektben.
3. **Build** (de ne futtassa) hello megoldás. Ha a rendszer kéri, állítsa vissza a NuGet-csomagok.
4. Használjon hello Azure portál tooupload egy [alkalmazáscsomag](batch-application-packages.md) a **PersistOutputsTask**. Hello tartalmaznak `PersistOutputsTask.exe` és a függő szerelvényeket hello .zip-csomagja, hello alkalmazás azonosítója túl "PersistOutputsTask" és az alkalmazás hello Csomagverzió túl "1.0".
5. **Start** (Futtatás) hello **PersistOutputs** projekt.
6. Amikor a kért toochoose hello adatmegőrzési technológia toouse futó hello minta be **2** toorun hello mintákkal hello Batch szolgáltatás API toopersist feladat kimenete.
7. Ha szükséges, újrafuttatása hello minta, írja be **3** toopersist kimeneti hello Batch szolgáltatás API, és is tooname hello tároló és a blob elérési utat célként toohello szabványos fájl konvenciók szerint.

## <a name="next-steps"></a>Következő lépések

- A .NET-hez lévő objektumán feladat kimenetének hello fájl egyezmények könyvtárhoz további információkért lásd: [megőrizni a feladat- és adatok tooAzure tárolási hello kötegelt fájl egyezmények könyvtárhoz tartozó .NET toopersist ](batch-task-output-file-conventions.md).
- Más megoldások a kimeneti adatait az Azure Batch információkért lásd: [Persist feladat- és kimeneti tooAzure tárolási](batch-task-output.md).

[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
