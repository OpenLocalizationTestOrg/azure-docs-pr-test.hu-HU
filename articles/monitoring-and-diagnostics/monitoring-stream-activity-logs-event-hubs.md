---
title: "aaaStream hello Azure tevékenységnapló tooEvent hubok |} Microsoft Docs"
description: "Ismerje meg, hogyan toostream hello Azure tevékenységnapló tooEvent hubok."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ec4c2d2c-8907-484f-a910-712403a06829
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 6/06/2017
ms.author: johnkem
ms.openlocfilehash: 336f92771b9d4379ad9dbcadc6997dfae7fae7bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="stream-hello-azure-activity-log-tooevent-hubs"></a><span data-ttu-id="33ef6-103">Adatfolyam-hello Azure tevékenységnapló tooEvent hubok</span><span class="sxs-lookup"><span data-stu-id="33ef6-103">Stream hello Azure Activity Log tooEvent Hubs</span></span>
<span data-ttu-id="33ef6-104">Hello [ **Azure tevékenységnapló** ](monitoring-overview-activity-logs.md) továbbítható a közel valós idejű tooany alkalmazások hello portálon hello beépített "Exportálás" beállítás használatával, vagy engedélyezésével hello a Service Bus szabályazonosító keresztül hello napló profil Az Azure PowerShell-parancsmagok vagy Azure CLI-t.</span><span class="sxs-lookup"><span data-stu-id="33ef6-104">hello [**Azure Activity Log**](monitoring-overview-activity-logs.md) can be streamed in near real time tooany application using hello built-in “Export” option in hello portal, or by enabling hello Service Bus Rule Id in a Log Profile via hello Azure PowerShell Cmdlets or Azure CLI.</span></span>

## <a name="what-you-can-do-with-hello-activity-log-and-event-hubs"></a><span data-ttu-id="33ef6-105">Mi mindent hello tevékenységnapló és az Event Hubs</span><span class="sxs-lookup"><span data-stu-id="33ef6-105">What you can do with hello Activity Log and Event Hubs</span></span>
<span data-ttu-id="33ef6-106">Az alábbiakban néhány módokon használhatja a streaming hello tevékenységnapló funkció hello:</span><span class="sxs-lookup"><span data-stu-id="33ef6-106">Here are just a few ways you might use hello streaming capability for hello Activity Log:</span></span>

* <span data-ttu-id="33ef6-107">**Adatfolyam-toothird-naplózás és telemetriai rendszerekre** – adott idő alatt, az Event Hubs streaming lesz a tevékenységnapló hello mechanizmus toopipe válnak a külső siem-ektől és jelentkezzen elemzési megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="33ef6-107">**Stream toothird-party logging and telemetry systems** – Over time, Event Hubs streaming will become hello mechanism toopipe your Activity Log into third-party SIEMs and log analytics solutions.</span></span>
* <span data-ttu-id="33ef6-108">**Egy egyéni telemetriai adatokat, illetve a naplózási platform** – Ha már rendelkezik egy egyedi telemetriai platform vagy a rendszer csak a gondolat egy kiválóan méretezhető hello közzétételi-feliratkozási épület jellegétől függően az Event Hubs lehetővé teszi a tooflexibly betöltési hello Tevékenységnapló.</span><span class="sxs-lookup"><span data-stu-id="33ef6-108">**Build a custom telemetry and logging platform** – If you already have a custom-built telemetry platform or are just thinking about building one, hello highly scalable publish-subscribe nature of Event Hubs allows you tooflexibly ingest hello activity log.</span></span> [<span data-ttu-id="33ef6-109">Dan Rosanova útmutató toousing Event Hubs egy globális méretű telemetriai platform itt talál.</span><span class="sxs-lookup"><span data-stu-id="33ef6-109">See Dan Rosanova’s guide toousing Event Hubs in a global scale telemetry platform here.</span></span>](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-hello-activity-log"></a><span data-ttu-id="33ef6-110">Hello tevékenységnapló adatfolyamként</span><span class="sxs-lookup"><span data-stu-id="33ef6-110">Enable streaming of hello Activity Log</span></span>
<span data-ttu-id="33ef6-111">Adatfolyamként való küldése a hello tevékenységnapló programozott módon vagy hello portálon keresztül is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="33ef6-111">You can enable streaming of hello Activity Log either programmatically or via hello portal.</span></span> <span data-ttu-id="33ef6-112">Mindkét módszer esetén, válasszon egy Service Bus Namespace és egy megosztott elérési házirendet a névtérhez, és az Eseményközpontok jön létre az adott névtérben, hello első új tevékenységnapló esemény bekövetkezésekor.</span><span class="sxs-lookup"><span data-stu-id="33ef6-112">Either way, you pick a Service Bus Namespace and a shared access policy for that namespace, and an Event Hub is created in that namespace when hello first new Activity Log event occurs.</span></span> <span data-ttu-id="33ef6-113">Ha nem rendelkezik egy Service Bus Namespace, először egy toocreate.</span><span class="sxs-lookup"><span data-stu-id="33ef6-113">If you do not have a Service Bus Namespace, you first need toocreate one.</span></span> <span data-ttu-id="33ef6-114">Ha korábban folyamatos átviteli tevékenységnapló események toothis Service Bus Namespace, korábban létrehozott Eseményközpont hello fogja használni.</span><span class="sxs-lookup"><span data-stu-id="33ef6-114">If you have previously streamed Activity Log events toothis Service Bus Namespace, hello Event Hub that was previously created will be reused.</span></span> <span data-ttu-id="33ef6-115">hello megosztott hozzáférési házirend meghatározása hello adatfolyam mechanizmussal rendelkezik hello engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="33ef6-115">hello shared access policy defines hello permissions that hello streaming mechanism has.</span></span> <span data-ttu-id="33ef6-116">Jelenleg tooan Event Hubs streaming van szükség **kezelése**, **küldése**, és **figyelésére** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="33ef6-116">Today, streaming tooan Event Hubs requires **Manage**, **Send**, and **Listen** permissions.</span></span> <span data-ttu-id="33ef6-117">Hozzon létre, vagy módosíthatja a Service Bus Namespace Service Bus Namespace megosztott hozzáférési házirendek a klasszikus portálon hello hello "Beállítása" lapon.</span><span class="sxs-lookup"><span data-stu-id="33ef6-117">You can create or modify Service Bus Namespace shared access policies in hello classic portal under hello “Configure” tab for your Service Bus Namespace.</span></span> <span data-ttu-id="33ef6-118">tooupdate hello tevékenységnapló napló profil tooinclude streaming, hello módosítás hello felhasználói hello ListKey engedéllyel kell rendelkeznie a Service Bus engedélyezési szabályt.</span><span class="sxs-lookup"><span data-stu-id="33ef6-118">tooupdate hello Activity Log log profile tooinclude streaming, hello user making hello change must have hello ListKey permission on that Service Bus Authorization Rule.</span></span>

<span data-ttu-id="33ef6-119">hello a busz- vagy event hub szolgáltatásnévtér nem rendelkezik a toobe hello hello előfizetési naplók kibocsátó mindaddig, amíg hello beállítás konfiguráló hello felhasználó rendelkezik-e megfelelő hozzáférési tooboth előfizetéseket a Szerepalapú tárolóként ugyanazt az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="33ef6-119">hello service bus or event hub namespace does not have toobe in hello same subscription as hello subscription emitting logs as long as hello user who configures hello setting has appropriate RBAC access tooboth subscriptions.</span></span>

### <a name="via-azure-portal"></a><span data-ttu-id="33ef6-120">Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="33ef6-120">Via Azure portal</span></span>
1. <span data-ttu-id="33ef6-121">Keresse meg a toohello **tevékenységnapló** panel bal oldalán található hello portal hello a hello menü segítségével.</span><span class="sxs-lookup"><span data-stu-id="33ef6-121">Navigate toohello **Activity Log** blade using hello menu on hello left side of hello portal.</span></span>
   
    ![Keresse meg a napló tooActivity portálon](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. <span data-ttu-id="33ef6-123">Kattintson a hello **exportálása** hello panel felső hello gombra.</span><span class="sxs-lookup"><span data-stu-id="33ef6-123">Click hello **Export** button at hello top of hello blade.</span></span>
   
    ![A portál Exportálás gomb](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. <span data-ttu-id="33ef6-125">Hello panelen megjelenő válassza ki, amelynek szeretné toostream események és hello Service Bus Namespace, amelyben az Eseményközpont toobe létrehozott az adatfolyamként történő ezeket az eseményeket szeretné hello régiók.</span><span class="sxs-lookup"><span data-stu-id="33ef6-125">In hello blade that appears, you can select hello regions for which you would like toostream events and hello Service Bus Namespace in which you would like an Event Hub toobe created for streaming these events.</span></span>
   
    ![Exportálás tevékenységnapló panel](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. <span data-ttu-id="33ef6-127">Kattintson a **mentése** toosave ezeket a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="33ef6-127">Click **Save** toosave these settings.</span></span> <span data-ttu-id="33ef6-128">hello-beállítások vannak azonnal kell alkalmazott tooyour előfizetés.</span><span class="sxs-lookup"><span data-stu-id="33ef6-128">hello settings are immediately be applied tooyour subscription.</span></span>

### <a name="via-powershell-cmdlets"></a><span data-ttu-id="33ef6-129">PowerShell-parancsmagok használatával</span><span class="sxs-lookup"><span data-stu-id="33ef6-129">Via PowerShell Cmdlets</span></span>
<span data-ttu-id="33ef6-130">Ha a napló-profilja már létezik, először tooremove adott profil.</span><span class="sxs-lookup"><span data-stu-id="33ef6-130">If a log profile already exists, you first need tooremove that profile.</span></span>

1. <span data-ttu-id="33ef6-131">Használjon `Get-AzureRmLogProfile` tooidentify, ha a napló-profil létezik</span><span class="sxs-lookup"><span data-stu-id="33ef6-131">Use `Get-AzureRmLogProfile` tooidentify if a log profile exists</span></span>
2. <span data-ttu-id="33ef6-132">Ha igen, használjon `Remove-AzureRmLogProfile` tooremove azt.</span><span class="sxs-lookup"><span data-stu-id="33ef6-132">If so, use `Remove-AzureRmLogProfile` tooremove it.</span></span>
3. <span data-ttu-id="33ef6-133">Használjon `Set-AzureRmLogProfile` toocreate profil:</span><span class="sxs-lookup"><span data-stu-id="33ef6-133">Use `Set-AzureRmLogProfile` toocreate a profile:</span></span>

```
Add-AzureRmLogProfile -Name my_log_profile -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

<span data-ttu-id="33ef6-134">Service Bus Szabályazonosító hello egy ilyen formátumú karakterláncot: {service bus erőforrás-azonosító} /authorizationrules/ {kulcsnév}, például</span><span class="sxs-lookup"><span data-stu-id="33ef6-134">hello Service Bus Rule ID is a string with this format: {service bus resource ID}/authorizationrules/{key name}, for example</span></span> 

### <a name="via-azure-cli"></a><span data-ttu-id="33ef6-135">Az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="33ef6-135">Via Azure CLI</span></span>
<span data-ttu-id="33ef6-136">Ha a napló-profilja már létezik, először tooremove adott profil.</span><span class="sxs-lookup"><span data-stu-id="33ef6-136">If a log profile already exists, you first need tooremove that profile.</span></span>

1. <span data-ttu-id="33ef6-137">Használjon `azure insights logprofile list` tooidentify, ha a napló-profil létezik</span><span class="sxs-lookup"><span data-stu-id="33ef6-137">Use `azure insights logprofile list` tooidentify if a log profile exists</span></span>
2. <span data-ttu-id="33ef6-138">Ha igen, használjon `azure insights logprofile delete` tooremove azt.</span><span class="sxs-lookup"><span data-stu-id="33ef6-138">If so, use `azure insights logprofile delete` tooremove it.</span></span>
3. <span data-ttu-id="33ef6-139">Használjon `azure insights logprofile add` toocreate profil:</span><span class="sxs-lookup"><span data-stu-id="33ef6-139">Use `azure insights logprofile add` toocreate a profile:</span></span>

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

<span data-ttu-id="33ef6-140">Service Bus Szabályazonosító hello egy ilyen formátumú karakterláncot: `{service bus resource ID}/authorizationrules/{key name}`.</span><span class="sxs-lookup"><span data-stu-id="33ef6-140">hello Service Bus Rule ID is a string with this format: `{service bus resource ID}/authorizationrules/{key name}`.</span></span>

## <a name="how-do-i-consume-hello-log-data-from-event-hubs"></a><span data-ttu-id="33ef6-141">Hogyan használnak a hello adatainak naplózása az Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="33ef6-141">How do I consume hello log data from Event Hubs?</span></span>
<span data-ttu-id="33ef6-142">[hello tevékenységnapló hello sémája érhető el itt](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="33ef6-142">[hello schema for hello Activity Log is available here](monitoring-overview-activity-logs.md).</span></span> <span data-ttu-id="33ef6-143">Minden esemény "rekordok." nevű JSON-blobok tömbje van</span><span class="sxs-lookup"><span data-stu-id="33ef6-143">Each event is in an array of JSON blobs called “records.”</span></span>

## <a name="next-steps"></a><span data-ttu-id="33ef6-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="33ef6-144">Next Steps</span></span>
* [<span data-ttu-id="33ef6-145">Archív hello tevékenységnapló tooa storage-fiók</span><span class="sxs-lookup"><span data-stu-id="33ef6-145">Archive hello Activity Log tooa storage account</span></span>](monitoring-archive-activity-log.md)
* [<span data-ttu-id="33ef6-146">Olvassa el a hello Azure tevékenységnapló hello áttekintése</span><span class="sxs-lookup"><span data-stu-id="33ef6-146">Read hello overview of hello Azure Activity Log</span></span>](monitoring-overview-activity-logs.md)
* [<span data-ttu-id="33ef6-147">Tevékenységnapló esemény alapján riasztás beállítása</span><span class="sxs-lookup"><span data-stu-id="33ef6-147">Set up an alert based on an Activity Log event</span></span>](insights-auditlog-to-webhook-email.md)

