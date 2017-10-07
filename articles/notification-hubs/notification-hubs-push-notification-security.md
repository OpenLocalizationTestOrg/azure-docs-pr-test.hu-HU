---
title: a Notification Hubs aaaSecurity
description: "Ez a témakör ismerteti az Azure notification hubs használatával biztonságát."
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: 6506177c-e25c-4af7-8508-a3ddca9dc07c
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: f59ad4594c2c0a2e2b22ab0b6d6bad53825a4dc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security"></a><span data-ttu-id="f8294-103">Biztonság</span><span class="sxs-lookup"><span data-stu-id="f8294-103">Security</span></span>
## <a name="overview"></a><span data-ttu-id="f8294-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="f8294-104">Overview</span></span>
<span data-ttu-id="f8294-105">Ez a témakör ismerteti az Azure Notification Hubs hello biztonsági modellt.</span><span class="sxs-lookup"><span data-stu-id="f8294-105">This topic describes hello security model of Azure Notification Hubs.</span></span> <span data-ttu-id="f8294-106">Mivel a Notification Hubs egy Szolgáltatásbusz-entitás, valósítják meg hello mint a Service Bus ugyanazt biztonsági modellt.</span><span class="sxs-lookup"><span data-stu-id="f8294-106">Because Notification Hubs are a Service Bus entity, they implement hello same security model as Service Bus.</span></span> <span data-ttu-id="f8294-107">További információkért lásd: hello [Service Bus hitelesítési](https://msdn.microsoft.com/library/azure/dn155925.aspx) témaköröket.</span><span class="sxs-lookup"><span data-stu-id="f8294-107">For more information, see hello [Service Bus Authentication](https://msdn.microsoft.com/library/azure/dn155925.aspx) topics.</span></span>

## <a name="shared-access-signature-security-sas"></a><span data-ttu-id="f8294-108">Megosztott hozzáférési aláírást biztonsági (SAS)</span><span class="sxs-lookup"><span data-stu-id="f8294-108">Shared Access Signature Security (SAS)</span></span>
<span data-ttu-id="f8294-109">A Notification Hubs megvalósítja az entitás szintű biztonsági rendszer SAS (közös hozzáférésű Jogosultságkód) néven ismert.</span><span class="sxs-lookup"><span data-stu-id="f8294-109">Notification Hubs implements an entity-level security scheme called SAS (Shared Access Signature).</span></span> <span data-ttu-id="f8294-110">Ez a séma lehetővé teszi, hogy az üzenetküldési entitások toodeclare too12 engedélyezési szabályok a leírásában, amely jogot adni, hogy az entitás.</span><span class="sxs-lookup"><span data-stu-id="f8294-110">This scheme enables messaging entities toodeclare up too12 authorization rules in their description that grant rights on that entity.</span></span>

<span data-ttu-id="f8294-111">Minden egyes szabály egy nevet, egy kulcs-érték (közös titkos kulcs) és olyan jogokkal, tartalmaz "Biztonsági jogcímeinek." hello szakaszban leírtak szerint</span><span class="sxs-lookup"><span data-stu-id="f8294-111">Each rule contains a name, a key value (shared secret), and a set of rights, as explained in hello section “Security Claims.”</span></span> <span data-ttu-id="f8294-112">Ha létrehoz egy értesítési központot, két szabály automatikusan létrejönnek: egy figyelési jogokkal (azaz a hello ügyfél alkalmazás által használt), és egy, az összes jogokkal (hello app háttér használ).</span><span class="sxs-lookup"><span data-stu-id="f8294-112">When creating a Notification Hub, two rules are automatically created: one with Listen rights (that hello client app uses) and one with all rights (that hello app backend uses).</span></span>

<span data-ttu-id="f8294-113">Végrehajtásakor regisztrációs felügyeleti ügyfél alkalmazásokból, ha hello információk keresztül küldött értesítések nem érzékeny (például időjárási frissítések), a közös módon tooaccess egy értesítési központot toogive hello kulcs értéke hello szabály-figyelési hozzáférés toohello ügyfélalkalmazás és toogive hello kulcs értéke hello szabály teljes körű hozzáférési toohello háttéralkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f8294-113">When performing registration management from client apps, if hello information sent via notifications is not sensitive (for example, weather updates), a common way tooaccess a Notification Hub is toogive hello key value of hello rule Listen-only access toohello client app, and toogive hello key value of hello rule full access toohello app backend.</span></span>

<span data-ttu-id="f8294-114">Nem ajánlott, hogy az ügyfél a Windows Áruházbeli alkalmazások beágyazása hello kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="f8294-114">It is not recommended that you embed hello key value in Windows Store client apps.</span></span> <span data-ttu-id="f8294-115">Egy módon tooavoid beágyazás hello kulcsérték van toohave hello ügyfélalkalmazás le azt hello háttéralkalmazás indításkor.</span><span class="sxs-lookup"><span data-stu-id="f8294-115">A way tooavoid embedding hello key value is toohave hello client app retrieve it from hello app backend at startup.</span></span>

<span data-ttu-id="f8294-116">Fontos, hogy a figyelési hozzáféréssel rendelkező kulcs hello toounderstand lehetővé teszi, hogy egy ügyfél app tooregister bármely címke.</span><span class="sxs-lookup"><span data-stu-id="f8294-116">It is important toounderstand that hello key with Listen access allows a client app tooregister for any tag.</span></span> <span data-ttu-id="f8294-117">Ha az alkalmazás korlátoznia kell a regisztrációkat toospecific címkék toospecific ügyfelek (például, ha a címkék képviselő felhasználói azonosítók), majd a háttéralkalmazás hello regisztrációk kell elvégeznie.</span><span class="sxs-lookup"><span data-stu-id="f8294-117">If your app must restrict registrations toospecific tags toospecific clients (for example, when tags represent user IDs), then your app backend must perform hello registrations.</span></span> <span data-ttu-id="f8294-118">További információkért tekintse meg a regisztrációs felügyeleti.</span><span class="sxs-lookup"><span data-stu-id="f8294-118">For more information, see Registration Management.</span></span> <span data-ttu-id="f8294-119">Ne feledje, hogy ezzel a módszerrel hello ügyfélalkalmazás fog nem közvetlen hozzáférést tooNotification hubok.</span><span class="sxs-lookup"><span data-stu-id="f8294-119">Note that in this way, hello client app will not have direct access tooNotification Hubs.</span></span>

## <a name="security-claims"></a><span data-ttu-id="f8294-120">Biztonsági jogcímek</span><span class="sxs-lookup"><span data-stu-id="f8294-120">Security claims</span></span>
<span data-ttu-id="f8294-121">Hasonló tooother entitások, értesítési központ műveletek végezhetők a három biztonsági jogcímek: figyelésére, küldése és kezelését.</span><span class="sxs-lookup"><span data-stu-id="f8294-121">Similar tooother entities, Notification Hub operations are allowed for three security claims: Listen, Send, and Manage.</span></span>

| <span data-ttu-id="f8294-122">Jogcím</span><span class="sxs-lookup"><span data-stu-id="f8294-122">Claim</span></span> | <span data-ttu-id="f8294-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="f8294-123">Description</span></span> | <span data-ttu-id="f8294-124">Engedélyezett műveletek</span><span class="sxs-lookup"><span data-stu-id="f8294-124">Operations allowed</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f8294-125">Figyelés</span><span class="sxs-lookup"><span data-stu-id="f8294-125">Listen</span></span> |<span data-ttu-id="f8294-126">Létrehozása/frissítése, olvassa el és egyetlen regisztráció törlése</span><span class="sxs-lookup"><span data-stu-id="f8294-126">Create/Update, Read, and Delete single registrations</span></span> |<span data-ttu-id="f8294-127">Regisztrációs létrehozása/frissítése</span><span class="sxs-lookup"><span data-stu-id="f8294-127">Create/Update registration</span></span><br><br><span data-ttu-id="f8294-128">Olvassa el a regisztrációs</span><span class="sxs-lookup"><span data-stu-id="f8294-128">Read registration</span></span><br><br><span data-ttu-id="f8294-129">Olvassa el az összes regisztrációját a számára egy.</span><span class="sxs-lookup"><span data-stu-id="f8294-129">Read all registrations for a handle</span></span><br><br><span data-ttu-id="f8294-130">Regisztráció törlése</span><span class="sxs-lookup"><span data-stu-id="f8294-130">Delete registration</span></span> |
| <span data-ttu-id="f8294-131">Küldés</span><span class="sxs-lookup"><span data-stu-id="f8294-131">Send</span></span> |<span data-ttu-id="f8294-132">Küldjön üzeneteket toohello értesítési központ</span><span class="sxs-lookup"><span data-stu-id="f8294-132">Send messages toohello notification hub</span></span> |<span data-ttu-id="f8294-133">Üzenet küldése</span><span class="sxs-lookup"><span data-stu-id="f8294-133">Send message</span></span> |
| <span data-ttu-id="f8294-134">Kezelés</span><span class="sxs-lookup"><span data-stu-id="f8294-134">Manage</span></span> |<span data-ttu-id="f8294-135">A Notification Hubs (beleértve a PNS hitelesítő adatokat, és a biztonsági kulcsok frissítése), és a címkék alapján olvasható regisztráció cRUDs</span><span class="sxs-lookup"><span data-stu-id="f8294-135">CRUDs on Notification Hubs (including updating PNS credentials, and security keys), and read registrations based on tags</span></span> |<span data-ttu-id="f8294-136">Létrehozás/frissítés/olvasás/törlés értesítési központok</span><span class="sxs-lookup"><span data-stu-id="f8294-136">Create/Update/Read/Delete notification hubs</span></span><br><br><span data-ttu-id="f8294-137">Olvassa el a regisztrációk címke szerint</span><span class="sxs-lookup"><span data-stu-id="f8294-137">Read registrations by tag</span></span> |

<span data-ttu-id="f8294-138">Notification hubs használatával a Microsoft Azure Access Control jogkivonatokat, és közvetlenül hello értesítési központ konfigurálva megosztott kulccsal rendelkező előállított aláírás jogkivonatokat kap jogcímeket elfogadni.</span><span class="sxs-lookup"><span data-stu-id="f8294-138">Notification Hubs accept claims granted by Microsoft Azure Access Control tokens, and by signature tokens generated with shared keys configured directly on hello Notification Hub.</span></span>
