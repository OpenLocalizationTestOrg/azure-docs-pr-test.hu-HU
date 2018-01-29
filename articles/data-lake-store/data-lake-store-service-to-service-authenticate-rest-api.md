---
title: "Szolgáltatások közötti hitelesítés: REST API-t a Data Lake Store az Azure Active Directoryval |} Microsoft Docs"
description: "Megtudhatja, hogyan szolgáltatások közötti hitelesítési elérése a Data Lake Store REST API használatával Azure Active Directory használatával"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: 80934d3e5ded5c01e473f8450a3484d84c46e94e
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/10/2018
---
# <a name="service-to-service-authentication-with-data-lake-store-using-rest-api"></a>Szolgáltatások közötti hitelesítés a Data Lake Store REST API használatával
> [!div class="op_single_selector"]
> * [A Java használata](data-lake-store-service-to-service-authenticate-java.md)
> * [A .NET SDK használata](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [A Python használata](data-lake-store-service-to-service-authenticate-python.md)
> * [A REST API használata](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
> 

Ebből a cikkből megismerheti a szolgáltatások közötti hitelesítést az Azure Data Lake Store REST API használatával kapcsolatos. A végfelhasználói hitelesítéshez a Data Lake Store REST API használatával, lásd: [végfelhasználói hitelesítési a Data Lake Store REST API használatával](data-lake-store-end-user-authenticate-rest-api.md).

## <a name="prerequisites"></a>Előfeltételek
* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).

* **Az Azure Active Directory "Web" alkalmazás létrehozása**. Végrehajtotta a lépéseket [szolgáltatások közötti hitelesítés az Azure Active Directory használatával a Data Lake Store](data-lake-store-service-to-service-authenticate-using-active-directory.md).

## <a name="service-to-service-authentication"></a>Szolgáltatások közötti hitelesítés
Ebben a forgatókönyvben az alkalmazás maga biztosítja saját hitelesítő adatait a műveletek végrehajtásához. Ehhez adja ki a következő kódrészletben látható hasonló POST kérelem: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

A kérelem kimenete egy engedélyezési jogkivonatot (kimaradásával `access-token` a kimenetben alább), hogy ezt követően adja meg a REST API-hívásokkal. A hitelesítési jogkivonat mentse a fájlt; szüksége lesz rájuk a Data Lake Store REST hívása esetén.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Ez a cikk a **nem interaktív** módszert alkalmazza. További információk a nem interaktív (szolgáltatások közötti) hívásokról: [Szolgáltatások közötti hívások hitelesítő adatok használatával](https://msdn.microsoft.com/library/azure/dn645543.aspx). 

## <a name="next-steps"></a>További lépések
Ebben a cikkben megtanulta, szolgáltatások közötti hitelesítés használata a hitelesítéshez az Azure Data Lake Store REST API használatával. Most már megtekintheti a következő cikkekben talál arról, hogyan működik az Azure Data Lake Store REST API használatával kommunikálhatnak.

* [A Data Lake Store REST API használatával fiókkezelési műveletekhez](data-lake-store-get-started-rest-api.md)
* [A Data Lake Store REST API használatával adatokat műveletek](data-lake-store-data-operations-rest-api.md)

