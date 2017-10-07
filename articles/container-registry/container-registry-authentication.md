---
title: "egy Azure-tárolót rendszerleíró aaaAuthenticate |} Microsoft Docs"
description: "Hogyan toolog tooan Azure tároló beállításjegyzék használatával egy Azure Active Directory szolgáltatás egyszerű vagy egy rendszergazdai fiók"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 128a937a-766a-415b-b9fc-35a6c2f27417
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a0b0462e8432b2567689debca322e2426baa7fa2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-a-private-docker-container-registry"></a>A hitelesítést egy titkos Docker-tároló beállításjegyzék
egy Azure-tárolót beállításjegyzék tároló képekkel toowork, jelentkezzen be hello `docker login` parancsot. Segítségével bejelentkezhet egy  **[Azure Active Directory szolgáltatás egyszerű](../active-directory/active-directory-application-objects.md)**  vagy a beállításjegyzék-specifikus **rendszergazdai fiók**. A cikkben az identitások további információkra.



## <a name="service-principal"></a>Egyszerű szolgáltatásnév

Is [rendelje hozzá egy egyszerű](container-registry-get-started-azure-cli.md#assign-a-service-principal) tooyour beállításjegyzék és a Docker egyszerű hitelesítést használni. Legtöbb esetben egy egyszerű szolgáltatás használata ajánlott. Hello alkalmazás Azonosítóját és jelszavát hello szolgáltatás egyszerű toohello `docker login` parancsban, ahogy az alábbi példa hello:

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Miután bejelentkezett, Docker hello hitelesítő adatokat, gyorsítótárazza, így nem kell tooremember hello alkalmazás azonosítója.

> [!TIP]
> Ha azt szeretné, újragenerálás hello jelszavát, amely egy egyszerű hello futtatásával `az ad sp reset-credentials` parancsot.
>


Engedélyezi szolgáltatás rendszerbiztonsági tagok [szerepkörön alapuló hozzáférés](../active-directory/role-based-access-control-configure.md) tooa beállításjegyzék. A következő szerepkörök választhatók:
  * (Csak hozzáférésre lekéréses)-olvasó.
  * Közreműködő (lekérési és leküldési).
  * Tulajdonos (lekéréses leküldéses és hozzárendelése szerepkörökhöz tooother felhasználók).

Névtelen hozzáférés az Azure-tároló nyilvántartó nem érhető el. A nyilvános képek használhatják [Docker Hub](https://docs.docker.com/docker-hub/).

Több szolgáltatásnevekről tooa beállításkulcs, amely lehetővé teszi a különböző felhasználók vagy alkalmazások toodefine hozzáférési rendelhet hozzá. Szolgáltatás rendszerbiztonsági tagoknak is engedélyezheti a "távfelügyeleti" kapcsolat tooa beállításjegyzék fejlesztői vagy DevOps forgatókönyvek használhatók, mint a következő példák hello:

  * A beállításjegyzék tooorchestration rendszerekből többek között a DC/OS, Docker Swarm és Kubernetes üzemelő tárolópéldányokat. Akkor is is lekéréses tároló nyilvántartó toorelated Azure-szolgáltatásokkal, mint [Tárolószolgáltatás](../container-service/index.yml), [App Service](../app-service/index.md), [kötegelt](../batch/index.md), [Service Fabric](/azure/service-fabric/), stb.

  * Folyamatos integrációt és telepítést megoldások (például a Visual Studio Team Services vagy Jenkins), amelyek tároló lemezképeket, és küldje le őket tooa beállításjegyzék.





## <a name="admin-account"></a>Rendszergazdai fiók
Az egyes beállításjegyzék hoz létre rendszergazdai fiók jön létre automatikusan. Alapértelmezés szerint hello fiók le van tiltva, de engedélyezheti és beállíthatja hello hitelesítő adatok, például a hello [portal](container-registry-get-started-portal.md#manage-registry-settings) vagy hello segítségével [Azure CLI 2.0 parancsok](container-registry-get-started-azure-cli.md#manage-admin-credentials). Minden rendszergazdai fiók kerül a két jelszavakkal, amelyek mindegyikét helyreállíthatja. hello két jelszavak teszik toomaintain kapcsolatok toohello beállításjegyzék segítségével egy jelszó hello újragenerálja más jelszót. Ha hello fiók engedélyezve van, hello felhasználói nevet és vagy jelszó toohello átadhatók `docker login` az egyszerű hitelesítés toohello beállításjegyzék parancsot. Példa:

```
docker login myregistry.azurecr.io -u myAdminName -p myPassword1
```

> [!IMPORTANT]
> hello rendszergazdai fiókot egy felhasználói tooaccess hello beállításjegyzékbeli, főleg tesztelési célokra tervezték. Nem ajánlott tooshare hello rendszergazdai fiók hitelesítő adataival többi felhasználójával. Minden felhasználó egyetlen felhasználó toohello beállításjegyzékbeli jelennek meg. Módosítása vagy a fiók letiltása letiltja az összes felhasználó számára hello hitelesítő adatok használata a beállításjegyzék elérésének.
>


### <a name="next-steps"></a>Következő lépések
* [Az első kép hello Docker parancssori felület használatával leküldéses](container-registry-get-started-docker-cli.md).
* Hello tároló beállításjegyzék Preview hitelesítéssel kapcsolatos további információkért lásd: hello [blogbejegyzés](https://blogs.msdn.microsoft.com/stevelasker/2016/11/17/azure-container-registry-user-accounts/).
