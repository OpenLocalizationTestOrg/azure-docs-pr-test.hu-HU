---
title: "aaaApplication vagy felhasználóspecifikus Marathon-szolgáltatás |} Microsoft Docs"
description: "Alkalmazás- vagy felhasználóspecifikus Marathon-szolgáltatás létrehozása"
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Tárolók, Marathon, mikroszolgáltatások, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 1e6f69ed64e113a3a059788a71ddb57b6d3ad8da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-or-user-specific-marathon-service"></a>Alkalmazás- vagy felhasználóspecifikus Marathon-szolgáltatás létrehozása
Az Azure tárolószolgáltatás előkonfigurált Apache Mesos és Marathon rendszerű főkiszolgálókat biztosít. Ezek lehetnek használt tooorchestrate hello fürt, de az alkalmazások nem toouse hello főkiszolgálók ajánlott erre a célra. Például Marathon konfigurációjának hello tökéletesítse maguk főkiszolgálók hello való bejelentkezés és igényel módosítása – Ez egyedi főkiszolgálók, kissé eltérő hello standard és a szükséges toobe tartozó és a felügyelt bátorítja egymástól függetlenül. Emellett egy csapat által igényelt hello konfigurációs nem feltétlenül hello optimális konfigurációját egy másik.

Ez a cikk azt ismertetjük, hogyan tooadd egy alkalmazás- vagy felhasználóspecifikus Marathon-szolgáltatás.

Ez a szolgáltatás tooa egyetlen felhasználóhoz vagy csoporthoz fog tartozni, mert azok szabad tooconfigure bármely olyan módon, hogy kívánják azt. Azure Tárolószolgáltatás is, biztosítja, hogy a hello szolgáltatás toorun folytatódik. Hello szolgáltatás sikertelen lesz, ha az Azure Tárolószolgáltatás újraindítja azt. Hello legtöbbször ennek meg nem is lehet észrevenni állásidő.

## <a name="prerequisites"></a>Előfeltételek
[Azure Tárolószolgáltatás-példányt telepítése](container-service-deployment.md) az orchestrator írja be a DC/OS és [győződjön meg arról, hogy az ügyfelek kapcsolódhatnak-e tooyour fürt](../container-service-connect.md). Emellett hello lépéseket követve.

[!INCLUDE [install hello DC/OS CLI](../../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a>Alkalmazás- vagy felhasználóspecifikus Marathon-szolgáltatás létrehozása
Először hozzon létre egy JSON-konfigurációs fájlt, amely meghatározza, hogy szeretné-e toocreate hello alkalmazásszolgáltatás hello nevét. Jelen példában használjuk `marathon-alice` hello keretrendszer neveként. Hello fájl mentése hasonlót `marathon-alice.json`:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

A következő használata hello DC/OS parancssori felület tooinstall hello Marathon-példányt a hello beállítások a konfigurációs fájlban:

```bash
dcos package install --options=marathon-alice.json marathon
```

Meg kell jelennie a `marathon-alice` szolgáltatás fut a DC/OS felhasználói felületének hello szolgáltatások lapján található. hello felhasználói felület lesz `http://<hostname>/service/marathon-alice/` Ha azt szeretné, hogy tooaccess közvetlenül.

## <a name="set-hello-dcos-cli-tooaccess-hello-service"></a>Adja meg a hello DC/OS parancssori felület tooaccess hello szolgáltatást
Konfigurálhatja a DC/OS parancssori felület tooaccess ezen új szolgáltatás által hello beállítása `marathon.url` tulajdonság toopoint toohello `marathon-alice` példány az alábbiak szerint:

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

Ellenőrizheti a, amely hello működik a parancssori felület Marathon melyik példányával `dcos config show` parancsot. Visszaállíthatja toousing a fő Marathon-szolgáltatás hello paranccsal `dcos config unset marathon.url`.

