---
title: "Azure virtuális gépek tooan rendelkezésre állási készlet hozzáadásának aaaSupportability |} Microsoft Docs"
description: "Támogatás az Azure virtuális gépek tooan meglévő rendelkezésre állási csoport hozzáadása."
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
ms.assetid: 
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/15/2017
ms.author: delhan
ms.openlocfilehash: dc2bd86b916f1d1a0a0d4c9e870df829434c96b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="supportability-of-adding-azure-vms-tooan-existing-availability-set"></a><span data-ttu-id="93d76-103">Támogatás az Azure virtuális gépek tooan meglévő rendelkezésre állási csoport hozzáadása</span><span class="sxs-lookup"><span data-stu-id="93d76-103">Supportability of adding Azure VMs tooan existing availability set</span></span>

<span data-ttu-id="93d76-104">Alkalmanként felmerülhet a korlátozások, amikor új virtuális gépek (VM) tooan meglévő rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="93d76-104">You may occasionally encounter limitations when you add new virtual machines (VMs) tooan existing availability set.</span></span> <span data-ttu-id="93d76-105">a következő diagram részletei mely kombinálhatja a Virtuálisgép-sorozat hello azonos rendelkezésre állási csoport hello.</span><span class="sxs-lookup"><span data-stu-id="93d76-105">hello following chart details which VM series you can mix in hello same availability set.</span></span>

<span data-ttu-id="93d76-106">Hello támogatási mátrix toomix különböző típusú virtuális gépek itt található:</span><span class="sxs-lookup"><span data-stu-id="93d76-106">Here is hello supportability matrix toomix different types of VMs:</span></span>

<span data-ttu-id="93d76-107">Adatsorozat & rendelkezésre állási csoport</span><span class="sxs-lookup"><span data-stu-id="93d76-107">Series & Availability Set</span></span>|<span data-ttu-id="93d76-108">Második virtuális gép</span><span class="sxs-lookup"><span data-stu-id="93d76-108">Second VM</span></span>|<span data-ttu-id="93d76-109">A</span><span class="sxs-lookup"><span data-stu-id="93d76-109">A</span></span>|<span data-ttu-id="93d76-110">Av2</span><span class="sxs-lookup"><span data-stu-id="93d76-110">Av2</span></span>|<span data-ttu-id="93d76-111">D</span><span class="sxs-lookup"><span data-stu-id="93d76-111">D</span></span>|<span data-ttu-id="93d76-112">Dv2</span><span class="sxs-lookup"><span data-stu-id="93d76-112">Dv2</span></span>|<span data-ttu-id="93d76-113">Dv3</span><span class="sxs-lookup"><span data-stu-id="93d76-113">Dv3</span></span>|
|---|---|---|---|---|---|---|
|<span data-ttu-id="93d76-114">Első virtuális gép</span><span class="sxs-lookup"><span data-stu-id="93d76-114">First VM</span></span>|||||||
|<span data-ttu-id="93d76-115">A</span><span class="sxs-lookup"><span data-stu-id="93d76-115">A</span></span>||<span data-ttu-id="93d76-116">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-116">OK</span></span>|<span data-ttu-id="93d76-117">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-117">OK</span></span>|<span data-ttu-id="93d76-118">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-118">OK</span></span>|<span data-ttu-id="93d76-119">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-119">OK</span></span>|<span data-ttu-id="93d76-120">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-120">OK</span></span>|
|<span data-ttu-id="93d76-121">Av2</span><span class="sxs-lookup"><span data-stu-id="93d76-121">Av2</span></span>||<span data-ttu-id="93d76-122">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-122">OK</span></span>|<span data-ttu-id="93d76-123">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-123">OK</span></span>|<span data-ttu-id="93d76-124">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-124">OK</span></span>|<span data-ttu-id="93d76-125">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-125">OK</span></span>|<span data-ttu-id="93d76-126">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-126">OK</span></span>|
|<span data-ttu-id="93d76-127">D</span><span class="sxs-lookup"><span data-stu-id="93d76-127">D</span></span>||<span data-ttu-id="93d76-128">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-128">OK</span></span>|<span data-ttu-id="93d76-129">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-129">OK</span></span>|<span data-ttu-id="93d76-130">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-130">OK</span></span>|<span data-ttu-id="93d76-131">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-131">OK</span></span>|<span data-ttu-id="93d76-132">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-132">OK</span></span>|
|<span data-ttu-id="93d76-133">Dv2</span><span class="sxs-lookup"><span data-stu-id="93d76-133">Dv2</span></span>||<span data-ttu-id="93d76-134">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-134">OK</span></span>|<span data-ttu-id="93d76-135">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-135">OK</span></span>|<span data-ttu-id="93d76-136">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-136">OK</span></span>|<span data-ttu-id="93d76-137">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-137">OK</span></span>|<span data-ttu-id="93d76-138">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-138">OK</span></span>|
|<span data-ttu-id="93d76-139">Dv3</span><span class="sxs-lookup"><span data-stu-id="93d76-139">Dv3</span></span>||<span data-ttu-id="93d76-140">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-140">OK</span></span>|<span data-ttu-id="93d76-141">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-141">OK</span></span>|<span data-ttu-id="93d76-142">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-142">OK</span></span>|<span data-ttu-id="93d76-143">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-143">OK</span></span>|<span data-ttu-id="93d76-144">OKÉ</span><span class="sxs-lookup"><span data-stu-id="93d76-144">OK</span></span>|

<span data-ttu-id="93d76-145">Minden más adatsorozat nem lehet a hello azonos rendelkezésre állási csoportban, mert azok megkövetelik, hogy egy adott hardverekhez.</span><span class="sxs-lookup"><span data-stu-id="93d76-145">All other series could not be in hello same availability set because they require a specific hardware.</span></span>
