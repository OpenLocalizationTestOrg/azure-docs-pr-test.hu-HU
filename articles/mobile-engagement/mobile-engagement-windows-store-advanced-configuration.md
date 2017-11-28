---
title: "aaaAdvanced konfigurációs az univerzális Windows alkalmazások Engagement SDK"
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
ms.openlocfilehash: 23bd05012bc25d438d8d4985a112280bed0292b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a><span data-ttu-id="c54d6-103">Speciális Windows univerzális alkalmazások Engagement SDK konfigurációja</span><span class="sxs-lookup"><span data-stu-id="c54d6-103">Advanced Configuration for Windows Universal Apps Engagement SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c54d6-104">Univerzális Windows</span><span class="sxs-lookup"><span data-stu-id="c54d6-104">Universal Windows</span></span>](mobile-engagement-windows-store-advanced-configuration.md)
> * [<span data-ttu-id="c54d6-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="c54d6-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="c54d6-106">iOS</span><span class="sxs-lookup"><span data-stu-id="c54d6-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="c54d6-107">Android</span><span class="sxs-lookup"><span data-stu-id="c54d6-107">Android</span></span>](mobile-engagement-android-advanced-configuration.md)
> 
> 

<span data-ttu-id="c54d6-108">Ez az eljárás ismerteti, hogyan tooconfigure Azure Mobile Engagement Android-alkalmazások különböző konfigurációs beállításait.</span><span class="sxs-lookup"><span data-stu-id="c54d6-108">This procedure describes how tooconfigure various configuration options for Azure Mobile Engagement Android apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c54d6-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c54d6-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="advanced-configuration"></a><span data-ttu-id="c54d6-110">Speciális konfiguráció</span><span class="sxs-lookup"><span data-stu-id="c54d6-110">Advanced configuration</span></span>
### <a name="disable-automatic-crash-reporting"></a><span data-ttu-id="c54d6-111">Automatikus összeomlási jelentések letiltása</span><span class="sxs-lookup"><span data-stu-id="c54d6-111">Disable automatic crash reporting</span></span>
<span data-ttu-id="c54d6-112">Bármikor letilthatja hello automatikus összeomlási jelentéskészítési funkció bekapcsolása.</span><span class="sxs-lookup"><span data-stu-id="c54d6-112">You can disable hello automatic crash reporting feature of Engagement.</span></span> <span data-ttu-id="c54d6-113">Majd amikor nem kezelt kivétel történik, a bevonási nincs semmi hatása.</span><span class="sxs-lookup"><span data-stu-id="c54d6-113">Then, when an unhandled exception occurs, Engagement does nothing.</span></span>

> [!WARNING]
> <span data-ttu-id="c54d6-114">Ha letiltja ezt a szolgáltatást, akkor az alkalmazás egy nem kezelt összeomlás esetén Engagement nem küld hello összeomlási **és** hello munkamenet és feladatok nem zárja be.</span><span class="sxs-lookup"><span data-stu-id="c54d6-114">If you disable this feature, then when an unhandled crash occurs in your app, Engagement does not send hello crash **AND** does not close hello session and jobs.</span></span>
> 
> 

<span data-ttu-id="c54d6-115">toodisable automatikus összeomlási reporting, attól függően, hogy hello módon deklarált meg azt a konfigurációs testreszabása:</span><span class="sxs-lookup"><span data-stu-id="c54d6-115">toodisable automatic crash reporting, customize your configuration depending on hello way you declared it:</span></span>

#### <a name="from-engagementconfigurationxml-file"></a><span data-ttu-id="c54d6-116">A `EngagementConfiguration.xml` fájl</span><span class="sxs-lookup"><span data-stu-id="c54d6-116">From `EngagementConfiguration.xml` file</span></span>
<span data-ttu-id="c54d6-117">Állítsa be a jelentés túl összeomlik miattuk`false` közötti `<reportCrash>` és `</reportCrash>` címkék.</span><span class="sxs-lookup"><span data-stu-id="c54d6-117">Set report crash too`false` between `<reportCrash>` and `</reportCrash>` tags.</span></span>

#### <a name="from-engagementconfiguration-object-at-run-time"></a><span data-ttu-id="c54d6-118">A `EngagementConfiguration` futási időben objektum</span><span class="sxs-lookup"><span data-stu-id="c54d6-118">From `EngagementConfiguration` object at run time</span></span>
<span data-ttu-id="c54d6-119">Állítsa be a jelentés összeomlási toofalse a EngagementConfiguration objektum használatával.</span><span class="sxs-lookup"><span data-stu-id="c54d6-119">Set report crash toofalse using your EngagementConfiguration object.</span></span>

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a><span data-ttu-id="c54d6-120">Tiltsa le a valós idejű reporting</span><span class="sxs-lookup"><span data-stu-id="c54d6-120">Disable real time reporting</span></span>
<span data-ttu-id="c54d6-121">Alapértelmezés szerint hello Engagement service-jelentéseken naplók valós időben.</span><span class="sxs-lookup"><span data-stu-id="c54d6-121">By default, hello Engagement service reports logs in real time.</span></span> <span data-ttu-id="c54d6-122">Ha az alkalmazás naplók gyakran, jelenti jobb toobuffer hello naplók és tooreport őket egyszerre rendszeres időközönként alapon.</span><span class="sxs-lookup"><span data-stu-id="c54d6-122">If your application reports logs frequently, it is better toobuffer hello logs and tooreport them all at once on a regular time basis.</span></span> <span data-ttu-id="c54d6-123">Ennek elnevezése "kapacitásnövelés mód".</span><span class="sxs-lookup"><span data-stu-id="c54d6-123">This is called “burst mode”.</span></span>

<span data-ttu-id="c54d6-124">toodo tehát hello metódus hívása:</span><span class="sxs-lookup"><span data-stu-id="c54d6-124">toodo so, call hello method:</span></span>

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

<span data-ttu-id="c54d6-125">hello argumentum értéke a **ezredmásodperc**.</span><span class="sxs-lookup"><span data-stu-id="c54d6-125">hello argument is a value in **milliseconds**.</span></span> <span data-ttu-id="c54d6-126">Bármikor tooreactivate hello valós idejű naplózás, bármilyen paraméter nélkül, vagy hello 0 értékű hello metódus hívható meg.</span><span class="sxs-lookup"><span data-stu-id="c54d6-126">Whenever you want tooreactivate hello real-time logging, call hello method without any parameter, or with hello 0 value.</span></span>

<span data-ttu-id="c54d6-127">Kapacitásnövelés mód némileg növeli a hello eszközakkumulátor élettartamának, azonban hatással van hello Engagement figyelő: minden munkamenetek és feladatok időtartama vannak kerekítve toohello kapacitásnövelés küszöbértéket (így munkamenetek és feladatok rövidebb, mint a hello kapacitásnövelés küszöbértéke nem lehet látható).</span><span class="sxs-lookup"><span data-stu-id="c54d6-127">Burst mode slightly increases hello battery life but has an impact on hello Engagement Monitor: all sessions and jobs duration are rounded toohello burst threshold (thus, sessions and jobs shorter than hello burst threshold may not be visible).</span></span> <span data-ttu-id="c54d6-128">A 30000 (30s) mint kapacitásnövelés küszöbérték használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="c54d6-128">We recommend using a burst threshold no longer than 30000 (30s).</span></span> <span data-ttu-id="c54d6-129">Mentett naplókat még korlátozott too300 elemek.</span><span class="sxs-lookup"><span data-stu-id="c54d6-129">Saved logs are limited too300 items.</span></span> <span data-ttu-id="c54d6-130">Ha a küldő túl hosszú, néhány naplók elveszhet.</span><span class="sxs-lookup"><span data-stu-id="c54d6-130">If sending is too long, you can lose some logs.</span></span>

> [!WARNING]
> <span data-ttu-id="c54d6-131">hello kapacitásnövelés küszöbértéke nem lehet 1-nél kisebb konfigurált tooa időszak második.</span><span class="sxs-lookup"><span data-stu-id="c54d6-131">hello burst threshold cannot be configured tooa period less than one second.</span></span> <span data-ttu-id="c54d6-132">Ha így tesz, a hello SDK hello hiba: a nyomkövetési jeleníti meg, és automatikusan visszaállítja az alapértelmezett érték toohello, nulla másodperc.</span><span class="sxs-lookup"><span data-stu-id="c54d6-132">If you do so, hello SDK shows a trace with hello error and automatically resets toohello default value, zero seconds.</span></span> <span data-ttu-id="c54d6-133">Az eseményindítók hello SDK tooreport hello naplózza valós idejű.</span><span class="sxs-lookup"><span data-stu-id="c54d6-133">This triggers hello SDK tooreport hello logs in real-time.</span></span>
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
