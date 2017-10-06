---
title: "Hibaelhárítás az Azure Active Directory-tevékenység tartalomcsomag hibákat naplózza |} Microsoft Docs"
description: "A tartalom-es számú hello Azure Active Directory-tevékenység pack és lépéseket rendszerhez készült toofix hibaüzenetek listáját biztosít számukra."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 325de65ff1572a2f8f8319c0a52350bda03af3de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a>A tartalomcsomag hibákat naplózza hibaelhárítása az Azure Active Directory-tevékenység 


A Power BI-tartalomcsomag hello használata az Azure Active Directory előzetes, esetén lehetséges, hogy futtassa a következő hibák hello: 

- [A frissítés nem sikerült](active-directory-reporting-troubleshoot-content-pack.md#refresh-failed) 
- [Nem sikerült tooupdate adatforrás hitelesítő adatai](active-directory-reporting-troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [Adatok importálása túl sokáig tart](active-directory-reporting-troubleshoot-content-pack.md#importing-of-data-is-taking-too-long) 
 
Ez a témakör információkat hello lehetséges okok és hogyan toofix ezeket a hibákat.
 
## <a name="refresh-failed"></a>A frissítés nem sikerült 
 
**Hogyan van illesztett hibára**: e-mailt a Power bi-ban vagy a hello frissítési előzmények "sikertelen" állapota. 


| Ok | Hogyan toofix |
| ---   | ---        |
| Frissítési hiba hibákat is okozhat, ha hello azon felhasználók hitelesítési adatait hello toohello tartalomcsomag kapcsolódás alaphelyzetbe állítása, de nem frissíthetők a hello tartalomcsomag hello hello kapcsolódási beállításait. | A Power bi-ban keresse meg a megfelelő hello dataset toohello Azure Active Directory-tevékenység naplók irányítópult (Azure Active Directory tevékenységi naplóit), válassza ki a frissítésütemezés és írja be az Azure AD hitelesítő adatait. |
| Frissítés az alapul szolgáló tartalomcsomag hello toodata problémák miatt sikertelen lehet. | A fájl egy támogatási jegy. További részletekért lásd: [hogyan támogatják a tooget az Azure Active Directory](active-directory-troubleshooting-support-howto.md).|
 
 
## <a name="failed-tooupdate-data-source-credentials"></a>Nem sikerült tooupdate adatforrás hitelesítő adatai 
 
**Hogyan van illesztett hibára**: A Power BI-ban toohello Azure Active Directory-tevékenység naplókat (preview) tartalomcsomag kapcsolódás esetén. 

| Ok | Hogyan toofix |
| ---   | ---        |
| hello csatlakozó felhasználó, sem globális rendszergazda, és nem biztonsági olvasó vagy a biztonsági rendszergazdával. | Olyan fiókot használjon, amely globális rendszergazda vagy egy biztonsági olvasó, vagy a biztonsági rendszergazda tooaccess hello tartalomcsomagok. |
| A bérlő nincs Premium-bérlő, vagy nem rendelkezik a fájl Premium licenccel rendelkező legalább egy felhasználót. | A fájl egy támogatási jegy. További részletekért lásd: [hogyan támogatják a tooget az Azure Active Directory](active-directory-troubleshooting-support-howto.md).|
 

 

## <a name="importing-of-data-is-taking-too-long"></a>Adatok importálása túl sokáig tart 
 
**Hogyan van illesztett hibára**: A Power bi-ban, ha csatlakozott a tartalomcsomag hello adatok importálási folyamat elindul tooprepare az Azure Active Directory tevékenységnapló irányítópult. Hello üzenet jelenik meg: "*... adatok importálása* "  

| Ok | Hogyan toofix |
| ---   | ---        |
| Attól függően, hogy a bérlő hello méretét Ez a lépés sikerült igénybe vehet néhány perc too30 perc múlva. | Csak türelemmel. Ha üdvözlőüzenetére nem változik tooshowing az Irányítópulton egy órán belül, adjon fájl egy támogatási jegy. További részletekért lásd: [hogyan támogatják a tooget az Azure Active Directory](active-directory-troubleshooting-support-howto.md).|

## <a name="next-steps"></a>Következő lépések

Kattintson a Power BI tartalomcsomag Azure Active Directory előzetes tooinstall hello [Itt](https://powerbi.microsoft.com/en-us/blog/azure-active-directory-meets-power-bi/).


