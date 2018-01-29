---
title: "Azure Batch API-k és eszközök fejlesztők számára | Microsoft Docs"
description: "Megismerheti az Azure Batch szolgáltatással történő megoldásfejlesztéshez elérhető API-kat és eszközöket."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: get-started-article
ms.date: 10/12/2017
ms.author: tamram
ms.openlocfilehash: a17856013c8db2e671b8f5201fbcc9223953ab6f
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/20/2017
---
# <a name="overview-of-batch-apis-and-tools"></a>A Batch API-k és eszközök áttekintése

A párhuzamos számítási feladatok Azure Batch használatával végzett feldolgozása általában programozott módon történik az egyik [Batch API-val](#batch-development-apis). Az Ön által készített ügyfélalkalmazások vagy szolgáltatások a Batch API-k használatával kommunikálhatnak a Batch szolgáltatással. A Batch API-kkal számítási csomópontok készletét, virtuális gépeket vagy felhőszolgáltatásokat hozhat létre és felügyelhet. Ezt követően pedig a feladatok és tevékenységek ütemezésével futtathatja őket ezeken a csomópontokon. 

Hatékonyan dolgozhat fel nagyméretű számítási feladatokat a szervezet számára, vagy szolgáltatási előtérrendszert nyújthat az ügyfeleknek, hogy feladatokat és tevékenységeket futtathassanak – igény szerint vagy ütemterv alapján – egyetlen, több száz vagy akár több ezer csomóponton. Az Azure Batch szolgáltatást a nagyobb munkafolyamatok részeként is felügyelheti olyan eszközökkel, mint például az [Azure Data Factory](../data-factory/v1/data-factory-data-processing-using-batch.md?toc=%2fazure%2fbatch%2ftoc.json).

> [!TIP]
> Amikor készen áll a Batch API megismerésére, az általa nyújtott szolgáltatások alaposabb elsajátítása érdekében tekintse meg [Az Azure Batch funkcióinak áttekintése](batch-api-basics.md) című témakört.
> 
> 

## <a name="azure-accounts-for-batch-development"></a>A Batch-fejlesztéshez szükséges Azure-fiókok
A Batch-megoldások fejlesztésekor a következő fiókokat fogja használni a Microsoft Azure-ban.

* **Azure-fiók és -előfizetés** – Ha még nincs Azure-előfizetése, aktiválhatja [Visual Studio-előfizetői előnyeit][msdn_benefits], vagy regisztrálhat egy [ingyenes Azure-fiókot][free_account]. Fiók létrehozásakor a rendszer egy alapértelmezett előfizetést hoz létre.
* **Batch-fiók** – Az Azure Batch-erőforrások, például a készletek, számítási csomópontok, feladatok és tevékenységek, egy Batch-fiókkal vannak társítva. Amikor az alkalmazás egy kérelmet továbbít a Batch szolgáltatás felé, a hitelesítést az Azure Batch-fiók, a fiók URL-címe és egy hozzáférési kulcs vagy Azure Active Directory-jogkivonat használatával hajtja végre a szolgáltatás. Az Azure Portalon vagy programozott módon [hozhat létre Batch-fiókot](batch-account-create-portal.md).
* **Storage-fiók** – A Batch beépített támogatást kínál az [Azure Storage][azure_storage] fájljainak használatához. Szinte mindegyik Batch-forgatókönyv az Azure Blob Storage-ot használja a tevékenységek által futtatott programok és feldolgozott adatok átmeneti tárolásához, valamint a tevékenységek által létrehozott kimeneti adatok tárolásához. A Storage-fiók létrehozásával kapcsolatban tekintse meg a [Tudnivalók az Azure Storage-fiókokról](../storage/common/storage-create-storage-account.md) című témakört.

## <a name="batch-service-apis"></a>A Batch szolgáltatás API-jai

Az alkalmazások és szolgáltatások közvetlen REST API-hívásokat hajthatnak végre, illetve a következő ügyfélkódtárak legalább egyikének használatával futtathatják és kezelhetik az Azure Batch számítási feladatait.

| API | API-referencia | Letöltés | Oktatóanyag | Kódminták | További információ |
| --- | --- | --- | --- | --- | --- |
| **Batch REST** |[docs.microsoft.com][batch_rest] |N/A |- |- | [Támogatott verziók](/rest/api/batchservice/batch-service-rest-api-versioning) |
| **Batch .NET** |[docs.microsoft.com][api_net] |[NuGet ][api_net_nuget] |[Oktatóanyag](batch-dotnet-get-started.md) |[GitHub][api_sample_net] | [Kibocsátási megjegyzések](http://aka.ms/batch-net-dataplane-changelog) |
| **Batch Python** |[readthedocs.io][api_python] |[PyPI][api_python_pypi] |[Oktatóanyag](batch-python-tutorial.md)|[GitHub][api_sample_python] | [Olvass el](https://github.com/Azure/azure-sdk-for-python/blob/master/doc/batch.rst) |
| **Batch Node.js** |[docs.microsoft.com][api_nodejs] |[npm][api_nodejs_npm] |[Oktatóanyag](batch-nodejs-get-started.md) |- | [Olvass el](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/batch) |
| **Batch Java** |[github.io][api_java] |[Maven][api_java_jar] |- |[Olvass el][api_sample_java] | [Olvass el](https://github.com/Azure/azure-batch-sdk-for-java)|

## <a name="batch-management-apis"></a>Batch Management API-k

A Batch Azure Resource Manager API-jai programozott hozzáférést biztosítanak a Batch-fiókokhoz. Ezen API-k használatával programozott módon kezelheti a Batch-fiókokat, a kvótákat és az alkalmazáscsomagokat.  

| API | API-referencia | Letöltés | Oktatóanyag | Kódminták |
| --- | --- | --- | --- | --- |
| **Batch Resource Manager REST** |[docs.microsoft.com][api_rest_mgmt] |N/A |- |[GitHub](https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts) |
| **Batch Resource Manager .NET** |[docs.microsoft.com][api_net_mgmt] |[NuGet ][api_net_mgmt_nuget] | [Oktatóanyag](batch-management-dotnet.md) |[GitHub][api_sample_net] |

## <a name="batch-command-line-tools"></a>A Batch parancssori eszközei

Ezek a parancssori eszközök ugyanazt a funkcionalitást biztosítják, mint a Batch szolgáltatás API-jai és a Batch Management API-k: 

* [Batch PowerShell-parancsmagok][batch_ps]: Az [Azure PowerShell](/powershell/azure/overview) modulban található Azure Batch-parancsmagokkal felügyelheti a Batch-erőforrásokat a PowerShell használatával.
* [Azure CLI 2.0](/cli/azure/overview): Az Azure parancssori felület (Azure CLI) egy többplatformos eszközkészlet, amely rendszerhéjparancsokat biztosít sok Azure-szolgáltatással, például a Batch szolgáltatással és a Batch Management szolgáltatással való interakcióhoz. Az Azure CLI a Batch szolgáltatással való használatával kapcsolatos további információkért lásd: [Batch-erőforrások kezelése az Azure CLI-vel](batch-cli-get-started.md).

## <a name="other-tools-for-application-development"></a>Egyéb alkalmazásfejlesztési eszközök

Íme néhány további eszköz, amelyek hasznosak lehetnek a Batch-alkalmazások és -szolgáltatások létrehozása és hibakeresése során.

* [Azure Portal][portal]: Batch-készleteket, -feladatokat és -tevékenységeket hozhat létre, figyelhet meg és törölhet az Azure Portalon. Megtekintheti ezen és más erőforrások állapotinformációit a feladatok futtatásakor, és fájlokat is letölthet a készletekben található számítási csomópontokról. Letöltheti például egy sikertelen feladat `stderr.txt` fájlját a hibaelhárítás során. Távoli asztali (RDP-) fájlokat is letölthet, amelyekkel bejelentkezhet a számítási csomópontokba.
* [Azure BatchLabs][batch_labs]: A BatchLabs egy ingyenes, számos funkcióval ellátott, önálló ügyféleszköz Azure Batch-alkalmazások létrehozásához, hibakereséséhez és monitorozásához. Töltse le a [telepítőcsomagot](https://azure.github.io/BatchLabs/) Mac, Linux vagy Windows rendszerre.
* [Microsoft Azure Storage Explorer][storage_explorer]: Bár nem kimondottan Azure Batch-eszköz, a Storage Explorer egy másik értékes eszköz a Batch-megoldások fejlesztésekor és hibakeresésekor.

## <a name="additional-resources"></a>További források

- A Batch-alkalmazás eseményeinek naplózásával kapcsolatos információkért lásd: [Események naplózása Batck-alkalmazások diagnosztikai kiértékeléséhez és figyeléséhez](batch-diagnostics.md). A Batch-szolgáltatás által létrehozott események referenciájáért lásd:[Batch-elemzés](batch-analytics.md).
- A számítási csomópontok környezeti változóival kapcsolatos információért lásd: [Azure Batch számítási csomópont környezeti változói](batch-compute-node-environment-variables.md).

## <a name="next-steps"></a>Következő lépések

* Olvassa el [Az Azure Batch funkcióinak áttekintése](batch-api-basics.md) című témakört, amely hasznos információkkal szolgál a Batch használatára készülőknek. A cikk a Batch szolgáltatás erőforrásainak, például a készleteknek, csomópontoknak, feladatoknak, tevékenységeknek és sok olyan API funkciónak a részletesebb információit tartalmazza, amelyeket a Batch-alkalmazás kiépítésekor használhat.
* [Ismerkedjen meg az Azure Batch .NET-es kódtárával](batch-dotnet-get-started.md), hogy megtudja, hogyan használhatja a C# nyelvet és a Batch .NET-es kódtárat egy egyszerű számítási feladat végrehajtásához egy általános Batch-munkafolyamattal. Ennek a cikknek az áttekintése legyen az egyik első lépés, amikor igyekszik elsajátítani a Batch használatát. Az oktatóanyagnak [Python-](batch-python-tutorial.md) és [Node.js-](batch-nodejs-get-started.md)verziója is elérhető.
* Töltse le a [GitHubon található kódmintákat][github_samples], hogy lássa, hogyan használható a C# és a Python a Batch eszközzel a mintául szolgáló számítási feladatok ütemezése és feldolgozása során.


[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: /dotnet/api/overview/azure/batch/client
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_rest_mgmt]: /rest/api/batchmanagement/
[api_net_mgmt]: /dotnet/api/overview/azure/batch/management
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: /nodejs/api/overview/azure/batch
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: /powershell/module/azurerm.batch/
[batch_rest]: /rest/api/batchservice/
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_labs]: https://azure.github.io/BatchLabs/
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com
