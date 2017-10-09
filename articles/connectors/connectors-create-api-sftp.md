---
title: "aaaLearn hogyan toouse hello SFTP-összekötőt a logic Apps alkalmazásait |} Microsoft Docs"
description: "Az Azure App service logic Apps alkalmazások létrehozása Csatlakozás tooSFTP API toosend fájlok és fogadása. Például hozzon létre, update, get vagy fájltörléssel különböző műveleteket hajthat végre."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 697eb8b0-4a66-40c7-be7b-6aa6b131c7ad
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/20/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 3f50570191c9b9339fe6584b9056b2549512b789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-sftp-connector"></a><span data-ttu-id="39300-105">Hello SFTP összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="39300-105">Get started with hello SFTP connector</span></span>
<span data-ttu-id="39300-106">Hello SFTP összekötő tooaccess egy SFTP fiók toosend használja, és fájlokat fogadni.</span><span class="sxs-lookup"><span data-stu-id="39300-106">Use hello SFTP connector tooaccess an SFTP account toosend and receive files.</span></span> <span data-ttu-id="39300-107">Például hozzon létre, update, get vagy fájltörléssel különböző műveleteket hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="39300-107">You can perform various operations such as create, update, get or delete files.</span></span>  

<span data-ttu-id="39300-108">toouse [a csatlakozókat](apis-list.md), először toocreate logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="39300-108">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="39300-109">Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="39300-109">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-toosftp"></a><span data-ttu-id="39300-110">Csatlakozás tooSFTP</span><span class="sxs-lookup"><span data-stu-id="39300-110">Connect tooSFTP</span></span>
<span data-ttu-id="39300-111">A Logic Apps alkalmazást férhetnek hozzá a szolgáltatáshoz, először jóvá kell toocreate egy *kapcsolat* toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="39300-111">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="39300-112">A [kapcsolat](connectors-overview.md) biztosít a logikai alkalmazás és egy másik szolgáltatás közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="39300-112">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-toosftp"></a><span data-ttu-id="39300-113">Egy kapcsolat tooSFTP létrehozása</span><span class="sxs-lookup"><span data-stu-id="39300-113">Create a connection tooSFTP</span></span>
> [!INCLUDE [Steps toocreate a connection tooSFTP](../../includes/connectors-create-api-sftp.md)]
> 
> 

## <a name="use-an-sftp-trigger"></a><span data-ttu-id="39300-114">Használja az SFTP eseményindító</span><span class="sxs-lookup"><span data-stu-id="39300-114">Use an SFTP trigger</span></span>
<span data-ttu-id="39300-115">Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény.</span><span class="sxs-lookup"><span data-stu-id="39300-115">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="39300-116">[További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="39300-116">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

<span data-ttu-id="39300-117">Ebben a példában hello **SFTP - amikor egy fájl hozzáadása vagy módosítása** eseményindító használt tooinitiate logic app munkafolyamat akkor, ha egy fájl hozzáadja vagy módosítás dátuma, az SFTP-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="39300-117">In this example, hello **SFTP - When a file is added or modified** trigger is used tooinitiate a logic app workflow when a file is added to, or modified on, an SFTP server.</span></span> <span data-ttu-id="39300-118">Is hozzáadhat olyan feltétel, amely ellenőrzi a hello hello új vagy módosított fájl tartalmát, és lehetővé teszi egy döntési tooextract hello fájl tartalmának jelzik, hogy azt ki kell nyerni hello tartalmak használata előtt.</span><span class="sxs-lookup"><span data-stu-id="39300-118">You also add a condition that checks hello contents of hello new or modified file, and makes a decision tooextract hello file if its contents indicate that it should be extracted before using hello contents.</span></span> <span data-ttu-id="39300-119">Végül adja hozzá egy fájl művelet tooextract hello tartalmát, és helyezze el hello kivont tartalom hello SFTP kiszolgálón egy mappában.</span><span class="sxs-lookup"><span data-stu-id="39300-119">Finally, add an action tooextract hello contents of a file, and place hello extracted contents in a folder on hello SFTP server.</span></span> 

<span data-ttu-id="39300-120">A vállalati tegyük fel új fájlok, amelyek megfelelnek a megrendelések használhat az eseményindító toomonitor az SFTP mappa.</span><span class="sxs-lookup"><span data-stu-id="39300-120">In an enterprise example, you could use this trigger toomonitor an SFTP folder for new files that represent customer orders.</span></span>  <span data-ttu-id="39300-121">SFTP-összekötő intézkedésre aztán használhatja például a **fájl tartalmának lekérdezése**, hello ahhoz, hogy további feldolgozás és a tárolás rendelések adatbázist tooget hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="39300-121">You could then use an SFTP connector action, such as **Get file content**, tooget hello contents of hello order for further processing and storage in an orders database.</span></span>

> [!INCLUDE [Steps toocreate an SFTP trigger](../../includes/connectors-create-api-sftp-trigger.md)]
> 
> 

## <a name="add-a-condition"></a><span data-ttu-id="39300-122">Feltétel hozzáadása</span><span class="sxs-lookup"><span data-stu-id="39300-122">Add a condition</span></span>
> [!INCLUDE [Steps tooadd a condition](../../includes/connectors-create-api-sftp-condition.md)]
> 
> 

## <a name="use-an-sftp-action"></a><span data-ttu-id="39300-123">SFTP-művelet használata</span><span class="sxs-lookup"><span data-stu-id="39300-123">Use an SFTP action</span></span>
<span data-ttu-id="39300-124">Egy művelet által definiált logikai alkalmazás hello munkafolyamat során.</span><span class="sxs-lookup"><span data-stu-id="39300-124">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="39300-125">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="39300-125">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps toocreate an SFTP action](../../includes/connectors-create-api-sftp-action.md)]
> 
> 

## <a name="connector-specific-details"></a><span data-ttu-id="39300-126">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="39300-126">Connector-specific details</span></span>

<span data-ttu-id="39300-127">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/sftpconnector/).</span><span class="sxs-lookup"><span data-stu-id="39300-127">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/sftpconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="39300-128">További összekötők</span><span class="sxs-lookup"><span data-stu-id="39300-128">More connectors</span></span>
<span data-ttu-id="39300-129">Lépjen vissza toohello [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="39300-129">Go back toohello [APIs list](apis-list.md).</span></span>
