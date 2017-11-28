---
title: "aaaCreate munkafolyamatok a sablonok – Azure Logic Apps |} Microsoft Docs"
description: "Első lépések – Azure Logic Apps sablonok tooconnect apps használatával gyorsan hozzon létre a munkafolyamatok és adatok integrálására."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 3656acfb-eefd-4e75-b5d2-73da56c424c9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: LADocs; klam
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0127523662a12076772b52968f1e2f9cbad72a1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-workflow-using-a-pre-built-template-or-pattern-tooget-started-quickly"></a><span data-ttu-id="88d9f-103">A munkafolyamat egy előre elkészített sablon vagy a minta tooget gyorsan konfigurálása</span><span class="sxs-lookup"><span data-stu-id="88d9f-103">Configure a workflow using a pre-built template or pattern tooget started quickly</span></span>

## <a name="what-are-logic-app-templates"></a><span data-ttu-id="88d9f-104">Mik azok a logic app sablonok</span><span class="sxs-lookup"><span data-stu-id="88d9f-104">What are logic app templates</span></span>
<span data-ttu-id="88d9f-105">A logic app-sablon egy előre létrehozott logikai alkalmazás használható tooquickly Ismerkedés a saját munkafolyamat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="88d9f-105">A logic app template is a pre-built logic app that you can use tooquickly get started creating your own workflow.</span></span> 

<span data-ttu-id="88d9f-106">Ezek a sablonok egy jó módszer toodiscover vannak, amelyek használatával logic apps építhetők különböző minták.</span><span class="sxs-lookup"><span data-stu-id="88d9f-106">These templates are a good way toodiscover various patterns that can be built using logic apps.</span></span> <span data-ttu-id="88d9f-107">Használhatja ezeket a sablonokat, mint-van, illetve szerkesztésükhöz toofit a forgatókönyvéhez.</span><span class="sxs-lookup"><span data-stu-id="88d9f-107">You can either use these templates as-is or modify them toofit your scenario.</span></span>

## <a name="overview-of-available-templates"></a><span data-ttu-id="88d9f-108">Az elérhető sablonok – áttekintés</span><span class="sxs-lookup"><span data-stu-id="88d9f-108">Overview of available templates</span></span>
<span data-ttu-id="88d9f-109">Nincsenek közzétett hello logic app platform számos elérhető sablonok.</span><span class="sxs-lookup"><span data-stu-id="88d9f-109">There are many available templates currently published in hello logic app platform.</span></span> <span data-ttu-id="88d9f-110">Néhány példa kategóriák, valamint a őket, összekötői hello típusú alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="88d9f-110">Some example categories, as well as hello type of connectors used in them, are listed below.</span></span>

### <a name="enterprise-cloud-templates"></a><span data-ttu-id="88d9f-111">Vállalati felhő sablonok</span><span class="sxs-lookup"><span data-stu-id="88d9f-111">Enterprise cloud templates</span></span>
<span data-ttu-id="88d9f-112">Sablonok, amelyek integrálják a Dynamics CRM-hez, a Salesforce, a mezőbe, a Azure Blob és a többi összekötőt, a vállalati felhő igényeinek.</span><span class="sxs-lookup"><span data-stu-id="88d9f-112">Templates that integrate Dynamics CRM, Salesforce, Box, Azure Blob, and other connectors for your enterprise cloud needs.</span></span> <span data-ttu-id="88d9f-113">Néhány példa arra, mi ezekkel végezhető-sablonjai tartalmazzák a érdeklődők rendszerezése és a vállalati fájladatok biztonsági mentésekor.</span><span class="sxs-lookup"><span data-stu-id="88d9f-113">Some examples of what can be done with these templates include organizing your leads and backing up your corporate file data.</span></span>

### <a name="enterprise-integration-pack-templates"></a><span data-ttu-id="88d9f-114">Vállalati integrációs felügyeleticsomag-sablonok</span><span class="sxs-lookup"><span data-stu-id="88d9f-114">Enterprise integration pack templates</span></span>
<span data-ttu-id="88d9f-115">VETER beállításait (ellenőrzése, kinyerési, átalakítási, kiegészítése, útvonal-) folyamatok, egy X12 EDI keresztül AS2-dokumentum, és átalakítás az tooXML, valamint X12 és AS2 üzenet kezelési fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="88d9f-115">Configurations of VETER (validate, extract, transform, enrich, route) pipelines, receiving an X12 EDI document over AS2 and transforming it tooXML, as well as X12 and AS2 message handling.</span></span>

### <a name="protocol-pattern-templates"></a><span data-ttu-id="88d9f-116">Protokoll mintát sablonok</span><span class="sxs-lookup"><span data-stu-id="88d9f-116">Protocol pattern templates</span></span>
<span data-ttu-id="88d9f-117">Ezeket a sablonokat a logic apps tartalmazó protokoll kombinációját, például a kérés-válasz HTTP, valamint a Integrációk keresztül FTP és az SFTP áll.</span><span class="sxs-lookup"><span data-stu-id="88d9f-117">These templates consist of logic apps that contain protocol patterns such as request-response over HTTP as well as integrations across FTP and SFTP.</span></span> <span data-ttu-id="88d9f-118">A ezek léteznek vagy alapjaként protokoll összetettebb létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="88d9f-118">Use these as they exist, or as a basis for creating more complex protocol patterns.</span></span>  

### <a name="personal-productivity-templates"></a><span data-ttu-id="88d9f-119">Egyéni hatékonyságnövelő sablonok</span><span class="sxs-lookup"><span data-stu-id="88d9f-119">Personal productivity templates</span></span>
<span data-ttu-id="88d9f-120">Minták toohelp személyes termelékenység növelése közé tartozik a sablonokat, napi Emlékeztető beállítása, fontos munkaelemek kapcsolja be a feladatlisták és lefelé tooa egyetlen felhasználói jóváhagyás lépés hosszadalmas feladatok automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="88d9f-120">Patterns toohelp improve personal productivity include templates that set daily reminders, turn important work items into to-do lists, and automate lengthy tasks down tooa single user approval step.</span></span>

### <a name="consumer-cloud-templates"></a><span data-ttu-id="88d9f-121">Fogyasztó felhő sablonok</span><span class="sxs-lookup"><span data-stu-id="88d9f-121">Consumer cloud templates</span></span>
<span data-ttu-id="88d9f-122">Egyszerű sablonokat, amelyekbe beépül az közösségi szolgáltatások, például a Twitter, Slackhez és e-mailek, végső soron képes a közösségi média kezdeményezések marketing megerősítése.</span><span class="sxs-lookup"><span data-stu-id="88d9f-122">Simple templates that integrate with social media services such as Twitter, Slack, and email, ultimately capable of strengthening social media marketing initiatives.</span></span> <span data-ttu-id="88d9f-123">Ide tartoznak például zavaros másolja, sablonok, amelyek segíthetnek a termelékenység növelése által hagyományosan ismétlődő feladatok mentik a fordított időt is.</span><span class="sxs-lookup"><span data-stu-id="88d9f-123">These also include templates such as cloudy copying, which can help increase productivity by saving time spent on traditionally repetitive tasks.</span></span> 

## <a name="how-toocreate-a-logic-app-using-a-template"></a><span data-ttu-id="88d9f-124">Hogyan toocreate egy logic app sablon segítségével</span><span class="sxs-lookup"><span data-stu-id="88d9f-124">How toocreate a logic app using a template</span></span>
<span data-ttu-id="88d9f-125">megkezdődött a logic app sablon segítségével tooget hello logic app designer kísérhet.</span><span class="sxs-lookup"><span data-stu-id="88d9f-125">tooget started using a logic app template, go into hello logic app designer.</span></span> <span data-ttu-id="88d9f-126">Ha hello designer meg egy meglévő logikai alkalmazás megnyitásával, hello logikai alkalmazás automatikusan betölti a Tervező nézetben.</span><span class="sxs-lookup"><span data-stu-id="88d9f-126">If you're entering hello designer by opening an existing logic app, hello logic app automatically loads in your designer view.</span></span> <span data-ttu-id="88d9f-127">Azonban ha egy új logikai alkalmazást hoz létre, látható az alábbi üdvözlő képernyőt.</span><span class="sxs-lookup"><span data-stu-id="88d9f-127">However, if you're creating a new logic app, you see hello screen below.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

<span data-ttu-id="88d9f-128">Ezen a képernyőn megadhatja egy üres logikai alkalmazást vagy egy előre elkészített sablon toostart.</span><span class="sxs-lookup"><span data-stu-id="88d9f-128">From this screen, you can either choose toostart with a blank logic app or a pre-built template.</span></span> <span data-ttu-id="88d9f-129">Ha hello sablonok valamelyikét választja, további információkkal szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="88d9f-129">If you select one of hello templates, you are provided with additional information.</span></span> <span data-ttu-id="88d9f-130">A jelen példában használjuk hello *Dropbox egy új fájl jön létre, amikor másolja tooOneDrive* sablont.</span><span class="sxs-lookup"><span data-stu-id="88d9f-130">In this example, we use hello *When a new file is created in Dropbox, copy it tooOneDrive* template.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

<span data-ttu-id="88d9f-131">Ha úgy dönt, hogy toouse hello sablon, csak kiválaszthatnak egy hello *ezzel a sablonnal* gombra.</span><span class="sxs-lookup"><span data-stu-id="88d9f-131">If you choose toouse hello template, just select hello *use this template* button.</span></span> <span data-ttu-id="88d9f-132">Meg kell adnia toosign a tooyour fiókok alapján melyik összekötők hello sablont használja.</span><span class="sxs-lookup"><span data-stu-id="88d9f-132">You'll be asked toosign in tooyour accounts based on which connectors hello template utilizes.</span></span> <span data-ttu-id="88d9f-133">Vagy, ha korábban már létrehozta a kapcsolatot az összekötők, válassza alább látható módon folytatódik.</span><span class="sxs-lookup"><span data-stu-id="88d9f-133">Or, if you've previously established a connection with these connectors, you can select continue as seen below.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

<span data-ttu-id="88d9f-134">Hello kapcsolat létrehozása és kijelölése után *továbbra is*, hello logikai alkalmazás Tervező nézetben nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="88d9f-134">After establishing hello connection and selecting *continue*, hello logic app opens in designer view.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

<span data-ttu-id="88d9f-135">Hello a fenti példában a hello helyzet sok sablonokkal néhány hello kötelező tulajdonság mező előfordulhat, hogy lehet a program belül hello összekötők; azonban néhány továbbra is szükség lehet egy érték mielőtt tooproperly hello logikai alkalmazás központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="88d9f-135">In hello example above, as is hello case with many templates, some of hello mandatory property fields may be filled out within hello connectors; however, some might still require a value before being able tooproperly deploy hello logic app.</span></span> <span data-ttu-id="88d9f-136">Ha néhány hello hiányzó mezők megadása nélkül próbálja toodeploy, értesítést is, egy hibaüzenet.</span><span class="sxs-lookup"><span data-stu-id="88d9f-136">If you try toodeploy without entering some of hello missing fields, you'll be notified with an error message.</span></span>

<span data-ttu-id="88d9f-137">Ha a tooreturn toohello sablon megjelenítő, válassza ki a hello *sablonok* hello felső navigációs sáv gombjára.</span><span class="sxs-lookup"><span data-stu-id="88d9f-137">If you wish tooreturn toohello template viewer, select hello *Templates* button in hello top navigation bar.</span></span> <span data-ttu-id="88d9f-138">Váltás hátsó toohello sablon megjelenítő, elvesznek a nem mentett végrehajtási.</span><span class="sxs-lookup"><span data-stu-id="88d9f-138">By switching back toohello template viewer, you lose any unsaved progress.</span></span> <span data-ttu-id="88d9f-139">Előzetes tooswitching sablon viewer programba, megjelenik egy figyelmeztető üzenet, amely értesíti erről.</span><span class="sxs-lookup"><span data-stu-id="88d9f-139">Prior tooswitching back into template viewer, you'll see a warning message notifying you of this.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-toodeploy-a-logic-app-created-from-a-template"></a><span data-ttu-id="88d9f-140">Hogyan toodeploy logikai alkalmazás létrehozása sablonból</span><span class="sxs-lookup"><span data-stu-id="88d9f-140">How toodeploy a logic app created from a template</span></span>
<span data-ttu-id="88d9f-141">Amennyiben az a sablon betöltése, és a kívánt módosításokat végzett, jelölje ki mentési gomb hello hello bal felső sarokban.</span><span class="sxs-lookup"><span data-stu-id="88d9f-141">Once you have loaded your template and made any desired changes, select hello save button in hello upper left corner.</span></span> <span data-ttu-id="88d9f-142">Ez menti, és teszi közzé a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="88d9f-142">This saves and publishes your logic app.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

<span data-ttu-id="88d9f-143">Ha hogyan tooadd további lépések sablonná meglévő logic app további információkat, például volna, vagy hogy általában módosítást kell végrehajtania, további információ: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="88d9f-143">If you would like more information on how tooadd more steps into an existing logic app template, or make edits in general, read more at [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

