---
title: "aaaUse Azure kötegelt API-k és eszközök toodevelop nagyméretű párhuzamos feldolgozást végző megoldások |} Microsoft Docs"
description: "További tudnivalók a hello API-k és eszközök megoldások hello Azure Batch szolgáltatás érhető el."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: tamram
ms.openlocfilehash: ca75a1a63b3e7e6b0805e79a63685bc49aaaca8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-batch-apis-and-tools"></a>A Batch API-k és eszközök áttekintése

Azure kötegelt párhuzamos munkaterhelések feldolgozása általában történik programozott módon hello egyikével [kötegelt API-k](#batch-development-apis). Az ügyfél alkalmazás vagy szolgáltatás hello kötegelt API-k toocommunicate használható hello Batch-szolgáltatás. A kötegelt API-k hello, létrehozása és kezelése a számítási csomópontok készleteinek virtuális gépek vagy felhőszolgáltatások. Feladatok és a feladatok toorun meg azokat a csomópontokat, majd ütemezhet. 

Nagyméretű munkaterhelések feldolgozni a szervezet számára, vagy adja meg a szolgáltatás előtérbeli tooyour ügyfelek, hogy futtathatók feladatok és feladatokhoz – igény szerint vagy ütemezett--a egy, több száz vagy akár több ezer hatékony. Az Azure Batch szolgáltatást a nagyobb munkafolyamatok részeként is felügyelheti olyan eszközökkel, mint például az [Azure Data Factory](../data-factory/data-factory-data-processing-using-batch.md?toc=%2fazure%2fbatch%2ftoc.json).

> [!TIP]
> Amikor készen áll a kötegelt API toohello toodig a hello részletesebb ismeretének jellemzőkkel biztosít, tekintse meg a hello [Batch funkcióinak áttekintése a fejlesztők](batch-api-basics.md).
> 
> 

## <a name="azure-accounts-for-batch-development"></a>A Batch-fejlesztéshez szükséges Azure-fiókok
Amikor kötegelt megoldások fejlesztése, a következő fiókokat a Microsoft Azure-ban hello fogja használni.

* **Azure-fiók és -előfizetés** – Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit][msdn_benefits], vagy regisztrálhat egy [ingyenes Azure-fiókot][free_account]. Fiók létrehozásakor a rendszer egy alapértelmezett előfizetést hoz létre.
* **Batch-fiók** – Az Azure Batch-erőforrások, például a készletek, számítási csomópontok, feladatok és tevékenységek, egy Batch-fiókkal vannak társítva. Ha az alkalmazás elleni hello Batch szolgáltatás kérést, hello Azure Batch-fiók neve, a hello hello fiók URL-CÍMÉT és a hívóbetű hello kérelem hitelesíti. Is [Batch-fiók létrehozása](batch-account-create-portal.md) a hello Azure-portálon.
* **Storage-fiók** – A Batch beépített támogatást kínál az [Azure Storage][azure_storage] fájljainak használatához. Szinte minden kötegelt forgatókönyv Azure Blob Storage tárolót használja, előkészítési hello programok, amelyek a feladatok futnak, és azokat feldolgozó hello adatok, és általuk létrehozott kimeneti adatok hello tárolására. a tárfiók toocreate lásd: [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).

## <a name="batch-service-apis"></a>A Batch szolgáltatás API-jai

Az alkalmazások és szolgáltatások is közvetlen REST API-hívás vagy egy vagy több ügyfél szalagtárak toorun következő hello és kezelhet a Azure Batch számítási feladatait.

| API | API-referencia | Letöltés | Oktatóanyag | Kódminták | További információ |
| --- | --- | --- | --- | --- | --- |
| **Batch REST** |[MSDN][batch_rest] |N/A |- |- | [Támogatott verziók](https://docs.microsoft.com/rest/api/batchservice/batch-service-rest-api-versioning) |
| **Batch .NET** |[docs.microsoft.com][api_net] |[NuGet ][api_net_nuget] |[Oktatóanyag](batch-dotnet-get-started.md) |[GitHub][api_sample_net] | [Kibocsátási megjegyzések](http://aka.ms/batch-net-dataplane-changelog) |
| **Batch Python** |[readthedocs.io][api_python] |[PyPI][api_python_pypi] |[Oktatóanyag](batch-python-tutorial.md)|[GitHub][api_sample_python] | [Olvass el](https://github.com/Azure/azure-sdk-for-python/blob/master/doc/batch.rst) |
| **Batch Node.js** |[github.io][api_nodejs] |[npm][api_nodejs_npm] |- |- | [Olvass el](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/batch) |
| **Batch Java** |[github.io][api_java] |[Maven][api_java_jar] |- |[Olvass el][api_sample_java] | [Olvass el](https://github.com/Azure/azure-batch-sdk-for-java)|

## <a name="batch-management-apis"></a>Batch Management API-k

hello Azure Resource Manager API-khoz, kötegelt tooBatch fiókok programozott hozzáférést biztosítanak. Ezen API-k használatával programozott módon kezelheti a Batch-fiókokat, a kvótákat és az alkalmazáscsomagokat.  

| API | API-referencia | Letöltés | Oktatóanyag | Kódminták |
| --- | --- | --- | --- | --- |
| **Batch Resource Manager REST** |[docs.microsoft.com][api_rest_mgmt] |N/A |- |[GitHub](https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts) |
| **Batch Resource Manager .NET** |[docs.microsoft.com][api_net_mgmt] |[NuGet ][api_net_mgmt_nuget] | [Oktatóanyag](batch-management-dotnet.md) |[GitHub][api_sample_net] |

## <a name="batch-command-line-tools"></a>A Batch parancssori eszközei

A parancssori eszközök biztosítanak hello ugyanezeket a funkciókat, mint a Batch szolgáltatás és a kötegelt API-val hello: 

* [PowerShell-parancsmagok kötegelt][batch_ps]: hello Azure Batch-parancsmagok a hello [Azure PowerShell](/powershell/azure/overview) modul lehetővé teszik a toomanage kötegelt erőforrások az PowerShell.
* [Az Azure CLI](/cli/azure/overview): hello Azure parancssori felület (CLI) a kommunikáció számos Azure-szolgáltatásokkal, beleértve a hello Batch szolgáltatás és a Batch szolgáltatás felületparancsokat biztosító platformfüggetlen eszközkészlet. Lásd: [kezelése kötegelt erőforrások az Azure CLI](batch-cli-get-started.md) kötegelt hello Azure CLI használatáról további információt.

## <a name="other-tools-for-application-development"></a>Egyéb alkalmazásfejlesztési eszközök

Íme néhány további eszköz, amelyek hasznosak lehetnek a Batch-alkalmazások és -szolgáltatások létrehozása és hibakeresése során.

* [Azure-portálon][portal]: hozhat létre, figyeléséhez és törölhet kötegelt készletek, feladatok és feladatokat a hello Azure portál kötegelt paneleken. Az ezekhez és más erőforrások hello állapot információk is megtekinthetők, amíg a feladatok futtatásához, és még letölteni fájlokat hello számítási csomópontok a készletek. Letöltheti például egy sikertelen feladat `stderr.txt` fájlját a hibaelhárítás során. Emellett letöltheti használt toolog toocompute csomópontok távoli asztal (RDP) fájlok.
* [Azure Batch Explorer][batch_explorer]: kötegelt Explorer hasonló kötegelt erőforrás felügyeleti funkciókat kínál, hello Azure-portálon, azonban a Windows megjelenítési alaprendszer (WPF) ügyfélalkalmazás önálló. Hello Batch .NET minta alkalmazásokban érhető el a [GitHub][github_samples], a Visual Studio 2015-öt vagy újabb összeállítani, és használhatja azt toobrowse és hello erőforrások kezelése a Batch-fiók, amíg a most kialakított és hibakeresése a kötegelt megoldások. Feladat megtekintése, a készlet és a feladat részletei, töltse le a fájlokat a számítási csomópontok és toonodes távoli csatlakozás kötegelt Explorer letöltheti a távoli asztal (RDP) fájlok használatával.
* [A Microsoft Azure Tártallózó][storage_explorer]: közben nem feltétlenül egy Azure Batch eszköz, a Tártallózó hello egy másik értékes eszközt toohave addig, amíg a fejlesztés és a kötegelt megoldások hibakeresés.

## <a name="additional-resources"></a>További források

- toolearn kapcsolatos események naplózása a kötegelt alkalmazásból, lásd: [diagnosztikai kipróbálási és a Batch-megoldások figyelési események naplózása](batch-diagnostics.md). A Batch szolgáltatás hello által kiváltott események referenciáért lásd: [kötegelt elemzés](batch-analytics.md).
- A számítási csomópontok környezeti változóival kapcsolatos információért lásd: [Azure Batch számítási csomópont környezeti változói](batch-compute-node-environment-variables.md).

## <a name="next-steps"></a>Következő lépések

* Olvasási hello [Batch funkcióinak áttekintése a fejlesztők](batch-api-basics.md), bárki toouse kötegelt előkészítése alapvető információkat. hello cikkben Batch szolgáltatás erőforrásokhoz, mint a készletek, a csomópontok, a feladatok, és a feladatok és a hello részletesebb információkat is használhatja a kötegelt kérelem felépítésekor sok API-funkciókat.
* [Hello Azure Batch könyvtár az első lépései a .NET-keretrendszerhez készült](batch-dotnet-get-started.md) toolearn hogyan toouse C# és hello Batch .NET könyvtár tooexecute egy egyszerű munkaterhelés közös kötegelt munkafolyamattal. Ez a cikk a hogyan toouse hello Batch szolgáltatás tanulása közben első nem egyike lehet. Szerepel továbbá egy [Python verzió](batch-python-tutorial.md) hello oktatóanyag.
* Töltse le a hello [minták a Githubon code] [ github_samples] toosee, hogyan lehet a C# mind a Python kommunikáljanak a kötegelt tooschedule és a folyamat minta munkaterhelések.
* Tekintse meg a hello [kötegelt képzési terv] [ learning_path] tooget felmérheti, hello erőforrások tooyou érhető el, ha Ön további kötegelt toowork.


[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_rest_mgmt]: https://docs.microsoft.com/\rest/api/batchmanagement/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: /powershell/resourcemanager/azurerm.batch/v2.7.0/azurerm.batch
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com
