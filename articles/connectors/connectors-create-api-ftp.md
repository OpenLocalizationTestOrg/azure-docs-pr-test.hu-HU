---
title: "aaaLearn hogyan toouse hello FTP-összekötőt a logic apps |} Microsoft Docs"
description: "Az Azure App service logic Apps alkalmazások létrehozása Csatlakozás tooFTP server toomanage a fájlokat. Például feltöltés különféle műveleteket hajtson végre, frissítése, lekérése és FTP-kiszolgáló fájlok törlése."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: d83c55fe-eb59-4b7b-a5ec-afac5c772616
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/22/2016
ms.author: mandia; ladocs
ms.openlocfilehash: a7020df2005ebb34fc569627ae0516b8528cc7a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-ftp-connector"></a><span data-ttu-id="dde68-105">Hello FTP-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="dde68-105">Get started with hello FTP connector</span></span>
<span data-ttu-id="dde68-106">Hello FTP összekötő toomonitor használja, kezeléséhez, és hozza létre az FTP-kiszolgálón fájlokat.</span><span class="sxs-lookup"><span data-stu-id="dde68-106">Use hello FTP connector toomonitor, manage and create files on an  FTP server.</span></span> 

<span data-ttu-id="dde68-107">toouse [a csatlakozókat](apis-list.md), először toocreate logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="dde68-107">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="dde68-108">Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="dde68-108">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-tooftp"></a><span data-ttu-id="dde68-109">Csatlakozás tooFTP</span><span class="sxs-lookup"><span data-stu-id="dde68-109">Connect tooFTP</span></span>
<span data-ttu-id="dde68-110">A Logic Apps alkalmazást férhetnek hozzá a szolgáltatáshoz, először jóvá kell toocreate egy *kapcsolat* toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="dde68-110">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="dde68-111">A [kapcsolat](connectors-overview.md) biztosít a logikai alkalmazás és egy másik szolgáltatás közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="dde68-111">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-tooftp"></a><span data-ttu-id="dde68-112">Egy kapcsolat tooFTP létrehozása</span><span class="sxs-lookup"><span data-stu-id="dde68-112">Create a connection tooFTP</span></span>
> [!INCLUDE [Steps toocreate a connection tooFTP](../../includes/connectors-create-api-ftp.md)]
> 
> 

## <a name="use-a-ftp-trigger"></a><span data-ttu-id="dde68-113">FTP-eseményindítók</span><span class="sxs-lookup"><span data-stu-id="dde68-113">Use a FTP trigger</span></span>
<span data-ttu-id="dde68-114">Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény.</span><span class="sxs-lookup"><span data-stu-id="dde68-114">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="dde68-115">[További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="dde68-115">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="dde68-116">hello FTP-összekötő számára szükséges, hello Internet elérhető-e, és az FTP-kiszolgáló konfigurálva toooperate passzív módra.</span><span class="sxs-lookup"><span data-stu-id="dde68-116">hello FTP connector requires an FTP server that  is accessible from hello Internet and is configured toooperate with PASSIVE mode.</span></span> <span data-ttu-id="dde68-117">Hello FTP összekötő is **implicit ftps-t (FTP szolgáltatás SSL-en keresztül) nem kompatibilis**.</span><span class="sxs-lookup"><span data-stu-id="dde68-117">Also, hello FTP connector is **not compatible with implicit FTPS (FTP over SSL)**.</span></span> <span data-ttu-id="dde68-118">hello FTP-összekötő csak explicit ftps-t (FTP szolgáltatás SSL-en keresztül) támogatja.</span><span class="sxs-lookup"><span data-stu-id="dde68-118">hello FTP connector only supports explicit FTPS (FTP over SSL).</span></span>  
> 
> 

<span data-ttu-id="dde68-119">Ebben a példában I bemutatja, hogyan toouse hello **FTP - amikor egy fájl hozzáadása vagy módosítása** indul el egy logic app munkafolyamat tooinitiate, ha egy fájl hozzá vagy módosítja az FTP-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="dde68-119">In this example, I will show you how toouse hello **FTP - When a file is added or modified** trigger tooinitiate a logic app workflow when a file is added to, or modified on, an FTP server.</span></span> <span data-ttu-id="dde68-120">A vállalati tegyük fel használhat az eseményindító toomonitor egy FTP-mappa új fájlokat, amelyek megfelelnek a rendeléseket az ügyfelektől.</span><span class="sxs-lookup"><span data-stu-id="dde68-120">In an enterprise example, you could use this trigger toomonitor an FTP folder for new files that represent orders from customers.</span></span>  <span data-ttu-id="dde68-121">Az FTP-összekötő művelet aztán használhatja például a **fájl tartalmának lekérdezése** hello ahhoz, hogy további feldolgozás és a rendeléseket adatbázis tárolási tooget hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="dde68-121">You could then use an FTP connector action such as **Get file content** tooget hello contents of hello order for further processing and storage in your orders database.</span></span>

1. <span data-ttu-id="dde68-122">Adja meg *ftp* hello keresőmezőbe hello logic apps designer válassza hello **FTP - amikor egy fájl hozzáadása vagy módosítása** eseményindító</span><span class="sxs-lookup"><span data-stu-id="dde68-122">Enter *ftp* in hello search box on hello logic apps designer then select hello **FTP - When a file is added or modified**  trigger</span></span>   
   <span data-ttu-id="dde68-123">![FTP eseményindító kép 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)</span><span class="sxs-lookup"><span data-stu-id="dde68-123">![FTP trigger image 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)</span></span>  
   <span data-ttu-id="dde68-124">Hello **amikor egy fájl hozzáadása vagy módosítása esetén** vezérlő megnyílik.</span><span class="sxs-lookup"><span data-stu-id="dde68-124">hello **When a file is added or modified** control opens up</span></span>  
   <span data-ttu-id="dde68-125">![FTP eseményindító kép 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)</span><span class="sxs-lookup"><span data-stu-id="dde68-125">![FTP trigger image 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)</span></span>  
2. <span data-ttu-id="dde68-126">Jelölje be hello **...**  hello vezérlőelem hello jobb oldalán található.</span><span class="sxs-lookup"><span data-stu-id="dde68-126">Select hello **...** located on hello right side of hello control.</span></span> <span data-ttu-id="dde68-127">Ekkor megnyílik a hello mappa példányválasztó vezérlő</span><span class="sxs-lookup"><span data-stu-id="dde68-127">This opens hello folder picker control</span></span>  
   <span data-ttu-id="dde68-128">![FTP eseményindító kép 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)</span><span class="sxs-lookup"><span data-stu-id="dde68-128">![FTP trigger image 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)</span></span>  
3. <span data-ttu-id="dde68-129">Jelölje be hello  **>**  (jobbra), és keresse meg a toofind hello mappa toomonitor szánt új vagy módosított fájlokat.</span><span class="sxs-lookup"><span data-stu-id="dde68-129">Select hello **>** (right arrow) and browse toofind hello folder that you want toomonitor for new or modified files.</span></span> <span data-ttu-id="dde68-130">Válasszon ki hello mappát, és figyelje meg, hello mappa megjelenik a hello **mappa** vezérlő.</span><span class="sxs-lookup"><span data-stu-id="dde68-130">Select hello folder and notice hello folder is now displayed in hello **Folder** control.</span></span>  
   <span data-ttu-id="dde68-131">![FTP eseményindító kép 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)</span><span class="sxs-lookup"><span data-stu-id="dde68-131">![FTP trigger image 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)</span></span>   

<span data-ttu-id="dde68-132">Ezen a ponton a Logic Apps alkalmazást van beállítva, amely akkor kezdődik, hello futását más eseményindítók és műveletek hello munkafolyamat amikor egy fájl módosítani vagy mappában létrehozott hello adott FTP eseményindító.</span><span class="sxs-lookup"><span data-stu-id="dde68-132">At this point, your logic app has been configured with a trigger that will begin a run of hello other triggers and actions in hello workflow when a file is either modified or created in hello specific FTP folder.</span></span> 

> [!NOTE]
> <span data-ttu-id="dde68-133">A működési logic app toobe legalább egy eseményindító és egy műveletet kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="dde68-133">For a logic app toobe functional, it must contain at least one trigger and one action.</span></span> <span data-ttu-id="dde68-134">Kövesse hello hello következő szakasz tooadd műveletet.</span><span class="sxs-lookup"><span data-stu-id="dde68-134">Follow hello steps in hello next section tooadd an action.</span></span>  
> 
> 

## <a name="use-a-ftp-action"></a><span data-ttu-id="dde68-135">Egy FTP művelettel</span><span class="sxs-lookup"><span data-stu-id="dde68-135">Use a FTP action</span></span>
<span data-ttu-id="dde68-136">Egy művelet által definiált logikai alkalmazás hello munkafolyamat során.</span><span class="sxs-lookup"><span data-stu-id="dde68-136">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="dde68-137">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="dde68-137">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

<span data-ttu-id="dde68-138">Most, hogy egy eseményindító jelentek meg, kövesse ezeket lépéseket tooadd egy műveletet, amely megkapja hello hello eseményindító által megtalált hello új vagy módosított fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="dde68-138">Now that you have added a trigger, follow these steps tooadd an action that will get hello contents of hello new or modified file found by hello trigger.</span></span>    

1. <span data-ttu-id="dde68-139">Válassza ki **+ új lépés** tooadd hello hello művelet tooget hello hello fájl tartalmának hello FTP-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="dde68-139">Select **+ New step** tooadd hello hello action tooget hello contents of hello file on hello FTP server</span></span>  
2. <span data-ttu-id="dde68-140">Jelölje be hello **művelet hozzáadása** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="dde68-140">Select hello **Add an action** link.</span></span>  
   <span data-ttu-id="dde68-141">![FTP-művelet kép 1](./media/connectors-create-api-ftp/ftp-action-1.png)</span><span class="sxs-lookup"><span data-stu-id="dde68-141">![FTP action image 1](./media/connectors-create-api-ftp/ftp-action-1.png)</span></span>  
3. <span data-ttu-id="dde68-142">Adja meg *FTP* minden műveletre toosearch kapcsolódó tooFTP.</span><span class="sxs-lookup"><span data-stu-id="dde68-142">Enter *FTP* toosearch for all actions related tooFTP.</span></span>
4. <span data-ttu-id="dde68-143">Válassza ki **FTP - fájl tartalmának lekérdezése** , hello művelet tootake, amikor egy új vagy módosított fájl hello FTP mappában található.</span><span class="sxs-lookup"><span data-stu-id="dde68-143">Select **FTP - Get file content**  as hello action tootake when a new or modified file is found in hello FTP folder.</span></span>      
   <span data-ttu-id="dde68-144">![FTP-művelet kép 2](./media/connectors-create-api-ftp/ftp-action-2.png)</span><span class="sxs-lookup"><span data-stu-id="dde68-144">![FTP action image 2](./media/connectors-create-api-ftp/ftp-action-2.png)</span></span>  
   <span data-ttu-id="dde68-145">Hello **fájl tartalmának lekérdezése** megnyílik szabályozzák.</span><span class="sxs-lookup"><span data-stu-id="dde68-145">hello **Get file content** control opens.</span></span> <span data-ttu-id="dde68-146">**Megjegyzés:**: akkor lesz felszólító tooauthorize a logic app tooaccess az FTP-kiszolgáló fiókját, ha még nem meg korábban.</span><span class="sxs-lookup"><span data-stu-id="dde68-146">**Note**: you will be prompted tooauthorize your logic app tooaccess your FTP server account if you have not done so previously.</span></span>  
   <span data-ttu-id="dde68-147">![FTP-művelet kép 3](./media/connectors-create-api-ftp/ftp-action-3.png)</span><span class="sxs-lookup"><span data-stu-id="dde68-147">![FTP action image 3](./media/connectors-create-api-ftp/ftp-action-3.png)</span></span>   
5. <span data-ttu-id="dde68-148">Jelölje be hello **fájl** vezérlő (hello szóköz alatt található **fájl***).</span><span class="sxs-lookup"><span data-stu-id="dde68-148">Select hello **File** control (hello white space located below **FILE***).</span></span> <span data-ttu-id="dde68-149">Itt akkor használhatja a hello különböző tulajdonságok fájlból hello új vagy módosított hello FTP-kiszolgálón található.</span><span class="sxs-lookup"><span data-stu-id="dde68-149">Here, you can use any of hello various properties from hello new or modified file found on hello FTP server.</span></span>  
6. <span data-ttu-id="dde68-150">Jelölje be hello **tartalom fájl** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="dde68-150">Select hello **File content** option.</span></span>  
   <span data-ttu-id="dde68-151">![FTP-művelet kép 4](./media/connectors-create-api-ftp/ftp-action-4.png)</span><span class="sxs-lookup"><span data-stu-id="dde68-151">![FTP action image 4](./media/connectors-create-api-ftp/ftp-action-4.png)</span></span>   
7. <span data-ttu-id="dde68-152">hello vezérlő frissül, amely azt jelzi, hogy hello **FTP - fájl tartalmának lekérdezése** művelet kap hello *tartalom fájl* hello új vagy módosított fájl hello FTP-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="dde68-152">hello control is updated, indicating that hello **FTP - Get file content** action will get hello *file content* of hello new or modified file on hello FTP server.</span></span>      
   <span data-ttu-id="dde68-153">![FTP-művelet kép 5](./media/connectors-create-api-ftp/ftp-action-5.png)</span><span class="sxs-lookup"><span data-stu-id="dde68-153">![FTP action image 5](./media/connectors-create-api-ftp/ftp-action-5.png)</span></span>     
8. <span data-ttu-id="dde68-154">Mentse a munkáját, majd adja hozzá a fájl toohello FTP mappa tootest a munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="dde68-154">Save your work then add a file toohello FTP folder tootest your workflow.</span></span>    

<span data-ttu-id="dde68-155">Ezen a ponton hello logikai alkalmazás lett konfigurálva egy eseményindító toomonitor egy mappát egy FTP-kiszolgáló és a kezdeményezési hello munkafolyamat Ha talál egy új fájl vagy a módosított fájlt hello FTP-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="dde68-155">At this point, hello logic app has been configured with a trigger toomonitor a folder on an FTP server and initiate hello workflow when it finds either a new file or a modified file on hello FTP server.</span></span> 

<span data-ttu-id="dde68-156">hello logikai alkalmazás is van konfigurálva legyen egy új vagy módosított fájl hello művelet tooget hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="dde68-156">hello logic app also has been configured with an action tooget hello contents of hello new or modified file.</span></span>

<span data-ttu-id="dde68-157">Mostantól hozzáadhatja azok egy másik művelet, például hello [SQL Server - sor beszúrása](connectors-create-api-sqlazure.md) művelet tooinsert hello SQL-adatbázistáblában szereplő hello új vagy módosított fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="dde68-157">You can now add another action such as hello [SQL Server - insert row](connectors-create-api-sqlazure.md) action tooinsert hello contents of hello new or modified file into a SQL database table.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="dde68-158">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="dde68-158">Connector-specific details</span></span>

<span data-ttu-id="dde68-159">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/ftpconnector/).</span><span class="sxs-lookup"><span data-stu-id="dde68-159">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/ftpconnector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="dde68-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dde68-160">Next Steps</span></span>
[<span data-ttu-id="dde68-161">Logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="dde68-161">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

