---
title: "Machine Learning REST API hibakódok aaaAzure |} Microsoft Docs"
description: "Az ezen hibakódok által az Azure Machine Learning webszolgáltatás művelet sikerült visszaadni."
keywords: 
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 0923074b-3728-439d-a1b8-8a7245e39be4
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 11/16/2016
ms.author: garye
ms.openlocfilehash: 9495c8ef16e684d3c8978bcd11747cf95227b091
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-rest-api-error-codes"></a>Gépi tanulási REST API-hibakódok
 
hello következő hibakódok visszatérő kiszolgálón az Azure Machine Learning webszolgáltatás művelet.
 
## <a name="badargument-http-status-code-400"></a>BadArgument (HTTP-állapotkód: 400)
 
Érvénytelen argumentum lett megadva.
 
Ez az osztály a hiba azt jelenti, hogy valahol megadott argumentum érvénytelen volt. Ennek oka az lehet, a hitelesítő adatok vagy az Azure storage toosomething toohello webszolgáltatás átadott helyét. Tekintse meg hello "hibakód" mezője hello "Részletek" szakasz toodiagnose mely megadott argumentum érvénytelen volt.
 
| Hibakód: | Felhasználói üzenet |
| ---------- |--------------|
| BadParameterValue | a megadott hello paraméter értéke nem felel meg a hello paraméter szabály hello paraméter |
| BadSubscriptionId | hello előfizetés-azonosító használt tooscore nincs hello hello erőforrás létezik egy |
| BadVersionCall | Érvénytelen verzió paramétert hello API hívása során: {0}. Ellenőrizze a hello API Súgó oldal információátadáshoz hello megfelelő verzióját, és próbálkozzon újra. |
| BatchJobInputsNotSpecified | a következő szükséges input(s) hello hello kérelem nem adta: {0}. Ellenőrizze, hogy az összes bemeneti adatok van megadva, és próbálkozzon újra. |
| BatchJobInputsTooManySpecified | hello kérelem több bemeneti mint hello szolgáltatásban meghatározott megadott. Elfogadott input(s) listája: {0}. Ellenőrizze, hogy az összes bemeneti adatok helyesen van megadva, és próbálkozzon újra. |
| BlobNameTooLong | A diagnosztikai kimenetet túl hosszú a megadott Azure-blobot. tárolási elérési útja: {0}. Rövidítse le a hello elérési utat, és próbálkozzon újra. |
| BlobNotFound | Nem lehet tooaccess hello megadott Azure blob - {0}.  Azure-hibaüzenet: {1}. |
| ContainerIsEmpty | Nincs az Azure storage-tároló neve lett megadva. Adjon meg egy érvényes tároló nevet, és próbálkozzon újra. |
| ContainerSegmentInvalid | Érvénytelen a tároló nevét. Adjon meg egy érvényes tároló nevet, és próbálkozzon újra. |
| ContainerValidationFailed | BLOB-tároló érvényesítése sikertelen, hiba: {0}. |
| DataTypeNotSupported | A megadott adattípus nem támogatott. Adjon meg érvényes adattípus(ok), majd próbálja meg újból. |
| DuplicateInputInBatchCall | hello kötegelt kérelem érvénytelen. Nem adható meg mind egyetlen és több bemeneti: hello azonos idő. Távolítsa el az elem hello kérelmet, és próbálkozzon újra. |
| ExpiryTimeInThePast | Elmúlt hello van megadva lejárati idő: {0}. Adjon meg egy jövőbeli lejárati időpontja (UTC), majd próbálja meg újból. toonever lejár, a lejárati idő tooNULL beállítása. |
| IncompleteSettings | Diagnosztikai beállítások nem teljesek. |
| InputBlobRelativeLocationInvalid | Nincs az Azure storage blob megadott név. Adjon meg egy érvényes blob nevet, és próbálkozzon újra. |
| InvalidBlob | Érvénytelen blob specification BLOB: {0}. Ellenőrizze a kapcsolati karakterlánc / relatív elérési út vagy SAS-token specification helyes-e, és próbálkozzon újra. |
| InvalidBlobConnectionString | az egyik hello bemeneti/kimeneti BLOB érvénytelen a megadott kapcsolati karakterlánc hello: {0}. Ennek javításához, és próbálkozzon újra. |
| InvalidBlobExtension | hello blobhivatkozást: {0} érvénytelen vagy hiányzó fájl kiterjesztése. Támogatott fájltípusok a kimeneti típus: "{1}". |
| InvalidInputNames | Érvénytelen bemeneti hello kérelemben megadott neve: {0}. Hello bemeneti adatok toohello megfelelő bemenetei leképezni, és próbálkozzon újra. |
| InvalidOutputOverrideName | Érvénytelen kimeneti felülbírálása név: {0}. hello szolgáltatás nem rendelkezik egy ilyen nevű kimeneti csomópont. Adjon meg egy megfelelő kimeneti csomópont neve toooverride (kis-és nagybetűk vonatkozik). |
| InvalidQueryParameter | Érvénytelen a lekérdezési paraméter a(z) "{0}". {1} |
| MissingInputBlobInformation | Az Azure storage-blob adatok hiányoznak. Adjon meg egy érvényes kapcsolati karakterláncot és a relatív elérési út vagy URI Azonosítóját, és próbálkozzon újra. |
| MissingJobId | Egyetlen feladat sem a megadott azonosító. Egy feladat azonosító érték érkezett vissza egy feladatot a hello küldött első alkalommal. Ellenőrizze a hello feladat azonosítója helyes-e, és próbálkozzon újra. |
| MissingKeys | Nincsenek kulcsok megadott, vagy egy elsődleges vagy másodlagos kulcs nincs megadva. |
| MissingModelPackage | Nincs modell csomagazonosító vagy a modell csomag megadott. Adjon meg egy érvényes modell csomagazonosító vagy csomag modell, és próbálkozzon újra. |
| MissingOutputOverrideSpecification | hello kérelemhez nincs megadva hello blob specification kimeneti felülbírálás {0}. Adjon meg egy érvényes blob helyet hello kérelemmel, vagy távolítsa el a hello kimeneti megadott, ha nincs hely felülbírálás van szükség. |
| MissingRequestInput | hello webszolgáltatás bemenete vár, de nincs bemenet lett megadva. Ellenőrizze, érvényes bemeneti adatok találhatók közzétett hello alapján bemeneti portok hello modellben, és próbálkozzon újra. |
| MissingRequiredGlobalParameters | Nem minden kötelező web service paraméter(ek) megadott. Ellenőrizze, hogy hello paraméterek várt, de hello modul(ok) helyes-e, és próbálkozzon újra. |
| MissingRequiredOutputOverrides | A kötelező toopass egy titkosított szolgáltatásvégpont meghívásakor kimeneti felülbírálásai összes hello szolgáltatás kimenetek. Hiányzik a kimenetek most felülbírálások: {0} |
| MissingWebServiceGroupId | A megadott azonosító nem webes szolgáltatási csoport. Adjon meg egy érvényes web service csoport azonosítója, és próbálkozzon újra. |
| MissingWebServiceId | Nincs webes szolgáltatás a megadott azonosító. Adjon meg egy érvényes webszolgáltatás azonosítója, és próbálkozzon újra. |
| MissingWebServicePackage | Nincs web Service csomag megadott. Adjon meg egy érvényes web service-csomag, és próbálkozzon újra. |
| MissingWorkspaceId | Nincs munkaterület megadott azonosító. Adjon meg egy érvényes munkaterület azonosítója, és próbálkozzon újra. |
| ModelConfigurationInvalid | Érvénytelen modell konfigurációs hello modell csomagban. Győződjön meg arról, hello modell konfigurációs kimeneti végpont definícióját, szabványos hiba végpont, tartalmazza, és szabványos végpont ki, és próbálkozzon újra. |
| ModelPackageIdInvalid | Érvénytelen modell csomag azonosítóját. Győződjön meg arról, hogy hello modell csomag azonosítója helyes-e, és próbálkozzon újra. |
| RequestBodyInvalid | Nincs kérés törzsében megadott vagy hiba hello kérelem törzsének deszerializálása során. |
| RequestIsEmpty | Kérelem nem biztosított. Adjon meg egy érvényes kérelmet, és próbálkozzon újra. |
| UnexpectedParameter | Nem várt paraméterek megadva. Ellenőrizze a paraméternevek helyesen írta-e, csak a várt paraméterek át lettek adva, majd próbálkozzon újra. |
| UnknownError | Ismeretlen hiba történt. |
| UserParameterInvalid | {0} |
| WebServiceConcurrentRequestRequirementInvalid | Nem módosítható az egyidejű kérelmek követelmények {0} webszolgáltatáshoz. |
| WebServiceIdInvalid | Érvénytelen webszolgáltatás megadott felügyeletiszolgáltatás-azonosító. Webes szolgáltatás azonosítója érvényes guid-nak kell lennie. |
| WebServiceTooManyConcurrentRequestRequirement | Nem lehet beállítani az egyidejűleg futtatható kérelmek követelmény toomore mint {0}. |
| WebServiceTypeInvalid | Érvénytelen webszolgáltatás szolgáltatástípus. Győződjön meg arról hello érvényes webes szolgáltatás típusának megfelelő, és próbálkozzon újra. Érvényes webes szolgáltatás típusa: {0}. |
 
## <a name="baduserargument-http-status-code-400"></a>BadUserArgument (HTTP-állapotkód: 400)
 
Érvénytelen a megadott argumentum.
 
| Hibakód: | Felhasználói üzenet |
| ---------- |--------------|
| InputMismatchError | A bemeneti adat nem felel meg a bemeneti porthoz séma. |
| InputParseError | Nem sikerült a bemeneti vektoros elemzése.  Ellenőrizze a bemeneti vektoros hello rendelkezik-e oszlopok és adattípusokat megfelelő számú hello.  További részletek: {0}. |
| MissingRequiredGlobalParameters | Hello webszolgáltatás által várt paraméterek hiányoznak. Ellenőrizze a várt hello webszolgáltatás által az összes szükséges hello paraméterek helyesek, és próbálkozzon újra. |
| UnexpectedParameter | Ellenőrizze, hogy csak hello szükséges hello webszolgáltatás által várt paraméterek át lettek adva, és próbálkozzon újra. |
| UserParameterInvalid | {0} |
 
## <a name="invalidoperation-http-status-code-400"></a>InvalidOperation (HTTP-állapotkód: 400)
 
hello kérelem hello a jelenlegi környezetben érvénytelen.
 
| Hibakód: | Felhasználói üzenet |
| ---------- |--------------|
| CannotStartJob | hello feladat nem indítható el, mert {0} állapotban van. |
| IncompatibleModel | hello modell nem kompatibilis a hello kérelem verzióját. hello kérelem verzióval csak egyetlen datatable kimeneti modellek. |
| MultipleInputsNotAllowed | hello modell nem teszi lehetővé több bemeneti adatok. |
 
## <a name="libraryexecutionerror-http-status-code-400"></a>LibraryExecutionError (HTTP-állapotkód: 400)
 
Modul végrehajtási belső könyvtár hibába ütközött.
 
 
## <a name="moduleexecutionerror-http-status-code-400"></a>ModuleExecutionError (HTTP-állapotkód: 400)
 
Modul végrehajtási hibát észlelt.
 
 
## <a name="webservicepackageerror-http-status-code-400"></a>WebServicePackageError (HTTP-állapotkód: 400)
 
Érvénytelen webszolgáltatás service-csomag. Ellenőrizze a hello web service csomag megadott helyességéről, majd próbálja újra.
 
| Hibakód: | Felhasználói üzenet |
| ---------- |--------------|
| FormatError | hello web service-csomag rosszul megformázva. Részletek: {0} |
| RuntimesError | hello web service csomag graph érvénytelen. Részletek: {0} |
| ValidationError | hello web service csomag graph érvénytelen. Részletek: {0} |
 
## <a name="unauthorized-http-status-code-401"></a>Jogosulatlan (HTTP-állapotkód 401-es)
 
Kérelme, mert jogosulatlan tooaccess erőforrás.
 
| Hibakód: | Felhasználói üzenet |
| ---------- |--------------|
| AdminRequestUnauthorized | Nem engedélyezett |
| ManagementRequestUnauthorized | Nem engedélyezett |
| ScoreRequestUnauthorized | Érvénytelen hitelesítő adatok. |
 
## <a name="notfound-http-status-code-404"></a>NotFound (HTTP-állapotkód: 404)
 
Az erőforrás nem található.
 
| Hibakód: | Felhasználói üzenet |
| ---------- |--------------|
| ModelPackageNotFound | A csomag nem található modell. Ellenőrizze a hello modell csomag azonosítója helyes-e, és próbálkozzon újra. |
| WebServiceIdNotFoundInWorkspace | A webszolgáltatás nem található a munkaterület alatt. Hello webServiceId és hello workspaceId között eltérés tapasztalható. Ellenőrizze a megadott hello webszolgáltatás hello munkaterület részét képezi, és próbálkozzon újra. |
| WebServiceNotFound | A webszolgáltatás nem található. Ellenőrizze a hello webszolgáltatás azonosítója helyes-e, és próbálkozzon újra. |
| WorkspaceNotFound | A munkaterület nem található. Ellenőrizze a hello munkaterület azonosítója helyes-e, és próbálkozzon újra. |
 
## <a name="requesttimeout-http-status-code-408"></a>RequestTimeout (HTTP-állapotkód 408)
 
hello művelet nem sikerült engedélyezett idő hello belül.
 
| Hibakód: | Felhasználói üzenet |
| ---------- |--------------|
| RequestCanceled | Hello ügyfél megszakította a kérelem. |
| ScoreRequestTimeout | Végrehajtási kérelem túllépte az időkorlátot. |
 
## <a name="conflict-http-status-code-409"></a>Ütközés (HTTP-állapotkód 409)
 
Erőforrás már létezik.
 
| Hibakód: | Felhasználói üzenet |
| ---------- |--------------|
| ModelOutputMetadataMismatch | Érvénytelen kimeneti paraméter neve. Próbálja meg hello metaadatok szerkesztő modul toorename oszlopok használatával, és próbálkozzon újra. |
 
## <a name="memoryquotaviolation-http-status-code-413"></a>MemoryQuotaViolation (HTTP-állapotkód: 413)
 
hello modell nagyobb volt rendelve tooit hello memóriakvótáját.
 
| Hibakód: | Felhasználói üzenet |
| ---------- |--------------|
| OutOfMemoryLimit | hello modell felhasznált hozzá lett sajátíthatja elérhetőnél több memória. Hello modell engedélyezett maximális memória {0} MB. Ellenőrizze, hogy a modell problémák. |
 
## <a name="internalerror-http-status-code-500"></a>InternalError (HTTP-állapotkód: 500)
 
Végrehajtási belső hibát észlelt.
 
| Hibakód: | Felhasználói üzenet |
| ---------- |--------------|
| AdminAuthenticationFailed |  |
| BackendArgumentError |  |
| BackendBadRequest |  |
| ClusterConfigBlobMisconfigured |  |
| ContainerProcessTerminatedWithSystemError | rendszer-hiba miatt összeomlott hello tároló folyamat |
| ContainerProcessTerminatedWithUnknownError | Ismeretlen hiba miatt összeomlott hello tároló folyamat |
| ContainerValidationFailed | BLOB-tároló érvényesítése sikertelen, hiba: {0}. |
| DeleteWebServiceResourceFailed |  |
| ExceptionDeserializationError |  |
| FailedGettingApiDocument |  |
| FailedStoringWebService |  |
| InvalidMemoryConfiguration | InvalidMemoryConfiguration, ConfigValue: {0} |
| InvalidResourceCacheConfiguration |  |
| InvalidResourceDownloadConfiguration |  |
| InvalidWebServiceResources |  |
| MissingTaskInstance | A megadott argumentumok. Győződjön meg arról, hogy érvényes argumentumok lettek adva, majd próbálja meg újból. |
| ModelPackageInvalid |  |
| ModuleExecutionFailed |  |
| ModuleLoadFailed |  |
| ModuleObjectCloneFailed |  |
| OutputConversionFailed |  |
| PortDataTypeNotSupported | Port azonosítója = {0} még nem támogatott adattípusú: {1}. |
| ResourceDownload |  |
| ResourceLoadFailed |  |
| ServiceUrisNotFound |  |
| SwaggerGeneration | A swagger létrehozása sikertelen, a részletek: {0} |
| UnexpectedScoreStatus |  |
| UnknownBackendErrorResponse |  |
| UnknownError |  |
| UnknownJobStatusCode | Ismeretlen feladat állapotkód: {0}. |
| UnknownModuleError |  |
| UpdateWebServiceResourceFailed |  |
| WebServiceGroupNotFound |  |
| WebServicePackageInvalid | InvalidWebServicePackage, részletek: {0} |
| WorkerAuthorizationFailed |  |
| WorkerUnreachable |  |
 
## <a name="internalerrorsystemlowonmemory-http-status-code-500"></a>InternalErrorSystemLowOnMemory (HTTP-állapotkód: 500)
 
Végrehajtási belső hibát észlelt. A rendszer kevés a memória. Kérjük, próbálkozzon újból.
 
 
## <a name="modelpackageformaterror-http-status-code-500"></a>ModelPackageFormatError (HTTP-állapotkód: 500)
 
Érvénytelen modell csomag. Ellenőrizze a megadott hello modell csomag helyességéről, majd próbálja újra.
 
 
## <a name="webservicepackageinternalerror-http-status-code-500"></a>WebServicePackageInternalError (HTTP-állapotkód: 500)
 
Érvénytelen webszolgáltatás service-csomag. Ellenőrizze a megadott hello webes csomag helyességéről, majd próbálja újra.
 
| Hibakód: | Felhasználói üzenet |
| ---------- |--------------|
| ModuleError | hello web service csomag graph érvénytelen. Részletek: {0} |
 
## <a name="initializingcontainers-http-status-code-503"></a>InitializingContainers (HTTP-állapotkód 503-as)
 
hello kérelem nem hajtható végre, mint hello tárolók inicializálás alatt.
 
 
## <a name="serviceunavailable-http-status-code-503"></a>ServiceUnavailable (HTTP-állapotkód 503-as)
 
Szolgáltatás átmenetileg nem érhető el.
 
| Hibakód: | Felhasználói üzenet |
| ---------- |--------------|
| NoMoreResources | Erőforrások nem érhetők el a kérelmet. |
| RequestThrottled | Kérelem {0} végpont lett csökkentve. hello maximális feldolgozási hello végpont {1}. |
| TooManyConcurrentRequests | Túl sok egyidejű kérés küldése. |
| TooManyHostsBeingInitialized | Túl sok állomások inicializálása hello azonos idő. Fontolja meg a sávszélesség-szabályozás / újrapróbálkozás. |
| TooManyHostsBeingInitializedPerModel | Túl sok állomások inicializálása hello azonos idő. Fontolja meg a sávszélesség-szabályozás / újrapróbálkozás. |
 
## <a name="gatewaytimeout-http-status-code-504"></a>GatewayTimeout (HTTP-állapotkód: 504)
 
hello művelet nem sikerült engedélyezett idő hello belül.
 
| Hibakód: | Felhasználói üzenet |
| ---------- |--------------|
| BackendInitializationTimeout | hello webes szolgáltatás inicializálása nem sikerült engedélyezett idő hello belül. |
| BackendScoreTimeout | hello web service kérelem végrehajtása nem sikerült engedélyezett idő hello belül. |
 
