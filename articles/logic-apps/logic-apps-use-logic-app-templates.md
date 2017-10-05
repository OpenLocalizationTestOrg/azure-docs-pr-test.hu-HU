---
title: "Munkafolyamatok létrehozása sablonból - Azure Logic Apps |} Microsoft Docs"
description: "Első lépések - sablonokkal hozhatók létre munkafolyamatok gyorsan Azure Logic Apps alkalmazások és adatok integrálására."
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
ms.openlocfilehash: 89272869f7dfaa34cbd2ad32d67dca0955e6158b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-workflow-using-a-pre-built-template-or-pattern-to-get-started-quickly"></a><span data-ttu-id="e3aa9-103">A munkafolyamat egy előre elkészített sablon vagy a minta segítségével gyorsan konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e3aa9-103">Configure a workflow using a pre-built template or pattern to get started quickly</span></span>

## <a name="what-are-logic-app-templates"></a><span data-ttu-id="e3aa9-104">Mik azok a logic app sablonok</span><span class="sxs-lookup"><span data-stu-id="e3aa9-104">What are logic app templates</span></span>
<span data-ttu-id="e3aa9-105">A logic app-sablon egy előre létrehozott logikai alkalmazás, amely segítségével gyorsan megismerkedhet a saját munkafolyamat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-105">A logic app template is a pre-built logic app that you can use to quickly get started creating your own workflow.</span></span> 

<span data-ttu-id="e3aa9-106">Egy jó módszer különböző mintákat, amelyek használatával logic apps építhetők felderítésére.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-106">These templates are a good way to discover various patterns that can be built using logic apps.</span></span> <span data-ttu-id="e3aa9-107">Használhatja ezeket a sablonokat,-, vagy módosítsa őket a forgatókönyvnek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-107">You can either use these templates as-is or modify them to fit your scenario.</span></span>

## <a name="overview-of-available-templates"></a><span data-ttu-id="e3aa9-108">Az elérhető sablonok – áttekintés</span><span class="sxs-lookup"><span data-stu-id="e3aa9-108">Overview of available templates</span></span>
<span data-ttu-id="e3aa9-109">Nincsenek közzétett a logic app platform számos elérhető sablonok.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-109">There are many available templates currently published in the logic app platform.</span></span> <span data-ttu-id="e3aa9-110">Néhány példa kategóriák, valamint azokat, összekötői típusú alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-110">Some example categories, as well as the type of connectors used in them, are listed below.</span></span>

### <a name="enterprise-cloud-templates"></a><span data-ttu-id="e3aa9-111">Vállalati felhő sablonok</span><span class="sxs-lookup"><span data-stu-id="e3aa9-111">Enterprise cloud templates</span></span>
<span data-ttu-id="e3aa9-112">Sablonok, amelyek integrálják a Dynamics CRM-hez, a Salesforce, a mezőbe, a Azure Blob és a többi összekötőt, a vállalati felhő igényeinek.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-112">Templates that integrate Dynamics CRM, Salesforce, Box, Azure Blob, and other connectors for your enterprise cloud needs.</span></span> <span data-ttu-id="e3aa9-113">Néhány példa arra, mi ezekkel végezhető-sablonjai tartalmazzák a érdeklődők rendszerezése és a vállalati fájladatok biztonsági mentésekor.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-113">Some examples of what can be done with these templates include organizing your leads and backing up your corporate file data.</span></span>

### <a name="enterprise-integration-pack-templates"></a><span data-ttu-id="e3aa9-114">Vállalati integrációs felügyeleticsomag-sablonok</span><span class="sxs-lookup"><span data-stu-id="e3aa9-114">Enterprise integration pack templates</span></span>
<span data-ttu-id="e3aa9-115">VETER beállításait (ellenőrzése, kinyerési, átalakítási, kiegészítése, útvonal-) folyamatok, egy X12 EDI keresztül AS2-dokumentum, és átalakítása a XML-, valamint X12 és AS2 üzenet kezelési fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-115">Configurations of VETER (validate, extract, transform, enrich, route) pipelines, receiving an X12 EDI document over AS2 and transforming it to XML, as well as X12 and AS2 message handling.</span></span>

### <a name="protocol-pattern-templates"></a><span data-ttu-id="e3aa9-116">Protokoll mintát sablonok</span><span class="sxs-lookup"><span data-stu-id="e3aa9-116">Protocol pattern templates</span></span>
<span data-ttu-id="e3aa9-117">Ezeket a sablonokat a logic apps tartalmazó protokoll kombinációját, például a kérés-válasz HTTP, valamint a Integrációk keresztül FTP és az SFTP áll.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-117">These templates consist of logic apps that contain protocol patterns such as request-response over HTTP as well as integrations across FTP and SFTP.</span></span> <span data-ttu-id="e3aa9-118">A ezek léteznek vagy alapjaként protokoll összetettebb létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-118">Use these as they exist, or as a basis for creating more complex protocol patterns.</span></span>  

### <a name="personal-productivity-templates"></a><span data-ttu-id="e3aa9-119">Egyéni hatékonyságnövelő sablonok</span><span class="sxs-lookup"><span data-stu-id="e3aa9-119">Personal productivity templates</span></span>
<span data-ttu-id="e3aa9-120">Minták egyéni hatékonyságnövelő javítása érdekében például sablonokat, napi Emlékeztető beállítása, fontos munkaelemek kapcsolja be a feladatlisták, és le egy egyetlen felhasználói jóváhagyás lépés hosszadalmas feladatok automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-120">Patterns to help improve personal productivity include templates that set daily reminders, turn important work items into to-do lists, and automate lengthy tasks down to a single user approval step.</span></span>

### <a name="consumer-cloud-templates"></a><span data-ttu-id="e3aa9-121">Fogyasztó felhő sablonok</span><span class="sxs-lookup"><span data-stu-id="e3aa9-121">Consumer cloud templates</span></span>
<span data-ttu-id="e3aa9-122">Egyszerű sablonokat, amelyekbe beépül az közösségi szolgáltatások, például a Twitter, Slackhez és e-mailek, végső soron képes a közösségi média kezdeményezések marketing megerősítése.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-122">Simple templates that integrate with social media services such as Twitter, Slack, and email, ultimately capable of strengthening social media marketing initiatives.</span></span> <span data-ttu-id="e3aa9-123">Ide tartoznak például zavaros másolja, sablonok, amelyek segíthetnek a termelékenység növelése által hagyományosan ismétlődő feladatok mentik a fordított időt is.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-123">These also include templates such as cloudy copying, which can help increase productivity by saving time spent on traditionally repetitive tasks.</span></span> 

## <a name="how-to-create-a-logic-app-using-a-template"></a><span data-ttu-id="e3aa9-124">Egy sablon használatával logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e3aa9-124">How to create a logic app using a template</span></span>
<span data-ttu-id="e3aa9-125">Első lépésként a logic app sablon segítségével, nyissa meg a logic app Tervező be.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-125">To get started using a logic app template, go into the logic app designer.</span></span> <span data-ttu-id="e3aa9-126">Ha egy meglévő logikai alkalmazás megnyitásával adta meg a Tervező, a logikai alkalmazás automatikusan betölti a Tervező nézetben.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-126">If you're entering the designer by opening an existing logic app, the logic app automatically loads in your designer view.</span></span> <span data-ttu-id="e3aa9-127">Azonban ha egy új logikai alkalmazást hoz létre, megjelenik az alábbi képernyőn.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-127">However, if you're creating a new logic app, you see the screen below.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

<span data-ttu-id="e3aa9-128">Ezen a képernyőn megadhatja egy üres logikai alkalmazást vagy egy előre elkészített sablon elindítása.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-128">From this screen, you can either choose to start with a blank logic app or a pre-built template.</span></span> <span data-ttu-id="e3aa9-129">Ha a sablonok valamelyikét választja, további információkkal szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-129">If you select one of the templates, you are provided with additional information.</span></span> <span data-ttu-id="e3aa9-130">A jelen példában használjuk a *Dropbox egy új fájl jön létre, amikor másolásához OneDrive* sablont.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-130">In this example, we use the *When a new file is created in Dropbox, copy it to OneDrive* template.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

<span data-ttu-id="e3aa9-131">Ha a sablon használatát választja, csak, jelölje be a *ezzel a sablonnal* gombra.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-131">If you choose to use the template, just select the *use this template* button.</span></span> <span data-ttu-id="e3aa9-132">Kéri bejelentkezni a partnereket a mely összekötőket a sablont használja.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-132">You'll be asked to sign in to your accounts based on which connectors the template utilizes.</span></span> <span data-ttu-id="e3aa9-133">Vagy, ha korábban már létrehozta a kapcsolatot az összekötők, válassza alább látható módon folytatódik.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-133">Or, if you've previously established a connection with these connectors, you can select continue as seen below.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

<span data-ttu-id="e3aa9-134">A kapcsolat létrehozása és kijelölése után *továbbra is*, megnyílik a logikai alkalmazás Tervező nézetben.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-134">After establishing the connection and selecting *continue*, the logic app opens in designer view.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

<span data-ttu-id="e3aa9-135">A fenti példában mint sok sablonok esetében néhány kötelező tulajdonság mező lehet, hogy tölti ki az összekötők; belül azonban néhány továbbra is szükség lehet egy érték előtt tudni megfelelően telepíteni a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-135">In the example above, as is the case with many templates, some of the mandatory property fields may be filled out within the connectors; however, some might still require a value before being able to properly deploy the logic app.</span></span> <span data-ttu-id="e3aa9-136">Ha megpróbálja telepíteni néhányat a hiányzó mezőket megadása nélkül, értesítést is, egy hibaüzenet.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-136">If you try to deploy without entering some of the missing fields, you'll be notified with an error message.</span></span>

<span data-ttu-id="e3aa9-137">Ha szeretne térni az sablon használatával, jelölje be a *sablonok* a felső navigációs sáv gombjára.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-137">If you wish to return to the template viewer, select the *Templates* button in the top navigation bar.</span></span> <span data-ttu-id="e3aa9-138">Váltás a sablon megjelenítő, elvesznek a nem mentett végrehajtási.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-138">By switching back to the template viewer, you lose any unsaved progress.</span></span> <span data-ttu-id="e3aa9-139">Váltás sablon viewer programba, mielőtt megjelenik egy figyelmeztető üzenet, amely értesíti erről.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-139">Prior to switching back into template viewer, you'll see a warning message notifying you of this.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-to-deploy-a-logic-app-created-from-a-template"></a><span data-ttu-id="e3aa9-140">Egy sablonból létrehozott logikai alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="e3aa9-140">How to deploy a logic app created from a template</span></span>
<span data-ttu-id="e3aa9-141">Amennyiben az a sablon betöltése, és a kívánt módosításokat végzett, válassza a Mentés gombra a bal felső sarokban.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-141">Once you have loaded your template and made any desired changes, select the save button in the upper left corner.</span></span> <span data-ttu-id="e3aa9-142">Ez menti, és teszi közzé a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e3aa9-142">This saves and publishes your logic app.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

<span data-ttu-id="e3aa9-143">Ha szeretne további tájékoztatást hozzáadása egy meglévő logic app-sablon a további lépéseket, vagy a Szerkesztés általában, további információ: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="e3aa9-143">If you would like more information on how to add more steps into an existing logic app template, or make edits in general, read more at [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

