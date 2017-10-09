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
# <a name="supportability-of-adding-azure-vms-tooan-existing-availability-set"></a>Támogatás az Azure virtuális gépek tooan meglévő rendelkezésre állási csoport hozzáadása

Alkalmanként felmerülhet a korlátozások, amikor új virtuális gépek (VM) tooan meglévő rendelkezésre állási csoportot. a következő diagram részletei mely kombinálhatja a Virtuálisgép-sorozat hello azonos rendelkezésre állási csoport hello.

Hello támogatási mátrix toomix különböző típusú virtuális gépek itt található:

Adatsorozat & rendelkezésre állási csoport|Második virtuális gép|A|Av2|D|Dv2|Dv3|
|---|---|---|---|---|---|---|
|Első virtuális gép|||||||
|A||OKÉ|OKÉ|OKÉ|OKÉ|OKÉ|
|Av2||OKÉ|OKÉ|OKÉ|OKÉ|OKÉ|
|D||OKÉ|OKÉ|OKÉ|OKÉ|OKÉ|
|Dv2||OKÉ|OKÉ|OKÉ|OKÉ|OKÉ|
|Dv3||OKÉ|OKÉ|OKÉ|OKÉ|OKÉ|

Minden más adatsorozat nem lehet a hello azonos rendelkezésre állási csoportban, mert azok megkövetelik, hogy egy adott hardverekhez.
