---
title: Azure RBAC aaaTroubleshoot |} Microsoft Docs
description: "Segítség problémák vagy a szerepköralapú hozzáférés-vezérlés erőforrások kapcsolatos kérdésekre."
services: azure-portal
documentationcenter: na
author: andredm7
manager: femila
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 15feced32d8459d90c4c246d335932f90e1fc91f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="role-based-access-control-troubleshooting"></a>Szerepköralapú hozzáférés-vezérlés hibaelhárítása

Ez a dokumentum a cikk kapcsolatos kérdésekre ad közös hello speciális hozzáférési jogok, amelyeket a szerepkörök, megállapításához, hogy milyen tooexpect használatával hello hello Azure-portálon a szerepkörök és hozzáférési problémák megoldása is. Ezek a szerepkörök az összes erőforrástípus terjed ki:

* Tulajdonos  
* Közreműködő  
* Olvasó  

Tulajdonos és közreműködő szerepkörrel rendelkező személyek mindkét rendelkezik teljes körű hozzáférési toohello felügyeleti tapasztalhat, de egy közreműködői nem adhat hozzáférést tooother felhasználókat vagy csoportokat. Részek lesznek még ennél is érdekesebb megoldást hello olvasó szerepkört, az, hogy az adott azt fogja szánjon némi időt. Lásd: hello [szerepköralapú hozzáférés-vezérlés a get-started cikk](role-based-access-control-configure.md) miként férhetnek hozzá az toogrant leírását.

## <a name="app-service-workloads"></a>App service munkaterhelések
### <a name="write-access-capabilities"></a>Írási képességek
Ha a felhasználó csak olvasási hozzáféréssel tooa egyetlen webalkalmazás biztosít, néhány funkció le vannak tiltva, hogy nem várt. a következő felügyeleti képességeket hello szükséges **írási** férnek hozzá a tooa web app (tulajdonos vagy közreműködő), és nem érhetők el a olyan írásvédett forgatókönyv.

* Parancsok (például a start, stop, stb.)
* Általános konfiguráció, a skálázási beállításokat, a biztonsági mentés beállításait és a figyelési beállítások például beállítások módosítása
* Közzétételi hitelesítő adatokat és más titkos adatokat, például az alkalmazásbeállítások és kapcsolati karakterláncok használata
* A folyamatos átviteli naplók
* Diagnosztikai naplók konfiguráció
* Konzol (parancssor)
* Aktív és a legutóbbi központi telepítéseket (helyi git folyamatos üzembe helyezés)
* Becsült töltött
* Webteszt
* Virtuális hálózat (csak látható tooa olvasó Ha egy virtuális hálózat már be lett állítva egy írási hozzáféréssel rendelkező felhasználónak).

Ha sem tudja már használni ezen csempék, akkor tooask a rendszergazda közreműködői hozzáférés toohello webalkalmazás.

### <a name="dealing-with-related-resources"></a>Kapcsolódó erőforrások kezelése
Webalkalmazások vannak bonyolítja néhány különböző erőforrások együttműködés hello jelenlétét. Íme néhány webhelyekkel tipikus erőforráscsoport:

![Webes alkalmazás erőforráscsoport](./media/role-based-access-control-troubleshooting/website-resource-model.png)

Ennek eredményeképpen ha valaki biztosít hozzáférést toojust hello webalkalmazás, nagy részét hello webhely panel az Azure-portálon hello hello funkciói le van tiltva.

Ezek az elemek szükség **írási** toohello hozzáférési **App Service-csomag** , amely megfelel a tooyour webhely:  

* Megtekintés hello webalkalmazás tartozó IP-címek (ingyenes vagy normál)  
* Skála configuration (a példányok, a virtuális gép méretét, az automatikus skálázási beállítások száma)  
* Kvóták (tároló, a sávszélesség, a CPU)  

Ezek az elemek szükség **írási** teljes hozzáférési toohello **erőforráscsoport** , amely tartalmazza a webhely:  

* SSL-tanúsítványok és kötések (hello a helyek közötti SSL-tanúsítványok megoszthatók ugyanazt az erőforráscsoportot és földrajzi helyhez)  
* A riasztási szabályok  
* automatikus skálázási beállításokat  
* Application insights összetevőinek  
* Webteszt  

## <a name="virtual-machine-workloads"></a>Virtuális gépek terheléséhez
Sokkal például a web apps, bizonyos funkcióinak hello virtuális gépek paneljét szükségük van írási toohello virtuális gép, illetve tooother erőforrások hello erőforráscsoportban.

Virtuális gépek a kapcsolódó tooDomain nevek, virtuális hálózatok, storage-fiókok és a riasztási szabályok.

Ezek az elemek szükség **írási** toohello hozzáférési **virtuális gép**:

* Végpontok  
* IP-címek  
* Lemezek  
* Bővítmények  

Szükséges **írási** hozzáférés tooboth hello **virtuális gép**, és hello **erőforráscsoport** (együtt hello nevét), hogy az informatikai van:  

* Rendelkezésre állási csoport  
* Elosztott terhelésű készlethez  
* A riasztási szabályok  

Ha sem tudja már használni ezen csempék, kérje meg a rendszergazdát, közreműködő hozzáférés toohello erőforráscsoport.

## <a name="see-more"></a>Részletek
* [Szerepköralapú hozzáférés-vezérlés](role-based-access-control-configure.md): első lépések az RBAC a hello Azure-portálon.
* [Beépített szerepkörök](role-based-access-built-in-roles.md): részletes információkat szolgáltatva hello szerepköröket, az RBAC szabványos tartalmazza.
* [Egyéni szerepkörök az Azure RBAC](role-based-access-control-custom-roles.md): megtudhatja, hogyan toocreate egyéni szerepkörök toofit hozzáférését kell.
* [Access módosítási előzményeit jelentés létrehozása](role-based-access-control-access-change-history-report.md): nyomon követjük, hogy az RBAC más szerepkörök hozzárendeléséről.

