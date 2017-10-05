---
title: "Speciális Windows univerzális alkalmazások Engagement SDK konfigurációja"
description: "Speciális konfigurációs beállítások, az Azure Mobile Engagement az univerzális Windows-alkalmazások"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6d85dd5d-ac07-43ba-bbe4-e91c3a17690b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 10/04/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: cb9454212c94cf65093219c3d24c71277ede7877
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a><span data-ttu-id="27677-103">Speciális Windows univerzális alkalmazások Engagement SDK konfigurációja</span><span class="sxs-lookup"><span data-stu-id="27677-103">Advanced Configuration for Windows Universal Apps Engagement SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="27677-104">Univerzális Windows</span><span class="sxs-lookup"><span data-stu-id="27677-104">Universal Windows</span></span>](mobile-engagement-windows-store-advanced-configuration.md)
> * [<span data-ttu-id="27677-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="27677-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="27677-106">iOS</span><span class="sxs-lookup"><span data-stu-id="27677-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="27677-107">Android</span><span class="sxs-lookup"><span data-stu-id="27677-107">Android</span></span>](mobile-engagement-android-advanced-configuration.md)
> 
> 

<span data-ttu-id="27677-108">Ez az eljárás ismerteti az Azure Mobile Engagement Android-alkalmazások beállításainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="27677-108">This procedure describes how to configure various configuration options for Azure Mobile Engagement Android apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="27677-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="27677-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="advanced-configuration"></a><span data-ttu-id="27677-110">Speciális konfiguráció</span><span class="sxs-lookup"><span data-stu-id="27677-110">Advanced configuration</span></span>
### <a name="disable-automatic-crash-reporting"></a><span data-ttu-id="27677-111">Automatikus összeomlási jelentések letiltása</span><span class="sxs-lookup"><span data-stu-id="27677-111">Disable automatic crash reporting</span></span>
<span data-ttu-id="27677-112">A jelentéskészítési funkció bekapcsolása automatikus összeomlási letilthatja.</span><span class="sxs-lookup"><span data-stu-id="27677-112">You can disable the automatic crash reporting feature of Engagement.</span></span> <span data-ttu-id="27677-113">Majd amikor nem kezelt kivétel történik, a bevonási nincs semmi hatása.</span><span class="sxs-lookup"><span data-stu-id="27677-113">Then, when an unhandled exception occurs, Engagement does nothing.</span></span>

> [!WARNING]
> <span data-ttu-id="27677-114">Ha letiltja ezt a szolgáltatást, akkor az alkalmazás egy nem kezelt összeomlás esetén Engagement nem küldi el a összeomlási **és** nem zárja be a munkamenet és feladatok.</span><span class="sxs-lookup"><span data-stu-id="27677-114">If you disable this feature, then when an unhandled crash occurs in your app, Engagement does not send the crash **AND** does not close the session and jobs.</span></span>
> 
> 

<span data-ttu-id="27677-115">Automatikus összeomlási jelentések letiltásához alakítható attól függően, hogy Ön deklarált azt módja:</span><span class="sxs-lookup"><span data-stu-id="27677-115">To disable automatic crash reporting, customize your configuration depending on the way you declared it:</span></span>

#### <a name="from-engagementconfigurationxml-file"></a><span data-ttu-id="27677-116">A `EngagementConfiguration.xml` fájl</span><span class="sxs-lookup"><span data-stu-id="27677-116">From `EngagementConfiguration.xml` file</span></span>
<span data-ttu-id="27677-117">Jelentés összeomlási beállítása `false` közötti `<reportCrash>` és `</reportCrash>` címkék.</span><span class="sxs-lookup"><span data-stu-id="27677-117">Set report crash to `false` between `<reportCrash>` and `</reportCrash>` tags.</span></span>

#### <a name="from-engagementconfiguration-object-at-run-time"></a><span data-ttu-id="27677-118">A `EngagementConfiguration` futási időben objektum</span><span class="sxs-lookup"><span data-stu-id="27677-118">From `EngagementConfiguration` object at run time</span></span>
<span data-ttu-id="27677-119">A jelentés összeomlási értéke HAMIS, a EngagementConfiguration objektum használatával.</span><span class="sxs-lookup"><span data-stu-id="27677-119">Set report crash to false using your EngagementConfiguration object.</span></span>

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a><span data-ttu-id="27677-120">Tiltsa le a valós idejű reporting</span><span class="sxs-lookup"><span data-stu-id="27677-120">Disable real time reporting</span></span>
<span data-ttu-id="27677-121">Alapértelmezés szerint a bevonási service-jelentéseken naplók valós időben.</span><span class="sxs-lookup"><span data-stu-id="27677-121">By default, the Engagement service reports logs in real time.</span></span> <span data-ttu-id="27677-122">Ha az alkalmazás naplók gyakran, érdemes meghajtóin a naplók és, hogy azok egyszerre rendszeres időközönként alapon.</span><span class="sxs-lookup"><span data-stu-id="27677-122">If your application reports logs frequently, it is better to buffer the logs and to report them all at once on a regular time basis.</span></span> <span data-ttu-id="27677-123">Ennek elnevezése "kapacitásnövelés mód".</span><span class="sxs-lookup"><span data-stu-id="27677-123">This is called “burst mode”.</span></span>

<span data-ttu-id="27677-124">Ehhez a metódus meghívására:</span><span class="sxs-lookup"><span data-stu-id="27677-124">To do so, call the method:</span></span>

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

<span data-ttu-id="27677-125">Az argumentum értéke a **ezredmásodperc**.</span><span class="sxs-lookup"><span data-stu-id="27677-125">The argument is a value in **milliseconds**.</span></span> <span data-ttu-id="27677-126">Amikor a valós idejű naplózási újraaktiválni kívánt, metódushívás a bármely paraméter nélkül, vagy a 0 értéket.</span><span class="sxs-lookup"><span data-stu-id="27677-126">Whenever you want to reactivate the real-time logging, call the method without any parameter, or with the 0 value.</span></span>

<span data-ttu-id="27677-127">Kapacitásnövelés mód némileg növeli az eszközakkumulátor élettartamának, de hatással van a bevonási figyelő: minden munkamenetek és feladatok időtartama a kapacitásnövelés küszöbértéket (így munkamenetek és feladatok rövidebb, mint a kapacitásnövelés küszöbértéket nem láthatják) vannak kerekítve.</span><span class="sxs-lookup"><span data-stu-id="27677-127">Burst mode slightly increases the battery life but has an impact on the Engagement Monitor: all sessions and jobs duration are rounded to the burst threshold (thus, sessions and jobs shorter than the burst threshold may not be visible).</span></span> <span data-ttu-id="27677-128">A 30000 (30s) mint kapacitásnövelés küszöbérték használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="27677-128">We recommend using a burst threshold no longer than 30000 (30s).</span></span> <span data-ttu-id="27677-129">Mentett naplókat 300 elemet korlátozódnak.</span><span class="sxs-lookup"><span data-stu-id="27677-129">Saved logs are limited to 300 items.</span></span> <span data-ttu-id="27677-130">Ha a küldő túl hosszú, néhány naplók elveszhet.</span><span class="sxs-lookup"><span data-stu-id="27677-130">If sending is too long, you can lose some logs.</span></span>

> [!WARNING]
> <span data-ttu-id="27677-131">Nem lehet konfigurálni a kapacitásnövelés küszöbértéket, egy kisebb, mint egy második.</span><span class="sxs-lookup"><span data-stu-id="27677-131">The burst threshold cannot be configured to a period less than one second.</span></span> <span data-ttu-id="27677-132">Ha így tesz, az SDK-t jeleníti meg a hiba: a nyomkövetés, és automatikusan visszaállítja az alapértelmezett érték nulla másodperc.</span><span class="sxs-lookup"><span data-stu-id="27677-132">If you do so, the SDK shows a trace with the error and automatically resets to the default value, zero seconds.</span></span> <span data-ttu-id="27677-133">Ezzel elindítja az SDK jelentheti a naplók valós időben.</span><span class="sxs-lookup"><span data-stu-id="27677-133">This triggers the SDK to report the logs in real-time.</span></span>
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
