---
title: "Azure AD Connect szinkronizálása: hello alapértelmezett konfigurációjának módosítása |} Microsoft Docs"
description: "Ajánlott eljárásokat biztosít az Azure AD Connect szinkronizálási szolgáltatás hello alapértelmezett konfiguráció módosítása."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 7638a031-1635-4942-94c3-fce8f09eed5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: aa05e935edd02c49c3c3fdc198b854f50327847c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-best-practices-for-changing-hello-default-configuration"></a>Azure AD Connect szinkronizálása: ajánlott eljárások a hello alapértelmezett konfiguráció módosítása
hello Ez a témakör célja toodescribe támogatott és módosítások tooAzure AD Connect szinkronizálási szolgáltatás nem támogatott.

hello konfigurációs hozta létre az Azure AD Connect "adott állapotban" működik a legtöbb környezetben, amely a helyszíni Active Directory szinkronizálása az Azure ad-val. Bizonyos esetekben célszerű bizonyos módosítások tooa konfigurációs toosatisfy egy adott kell szükséges tooapply vagy követelmény.

## <a name="changes-toohello-service-account"></a>Módosítások toohello szolgáltatásfiók
Azure AD Connect szinkronizálása hello telepítési varázslóval létrehozott szolgáltatásfiók alatt fut. A szolgáltatásfiók rendelkezik hello titkosítási kulcsok toohello által használt adatbázis szinkronizálása. Létrejön egy 127 karakter hosszú jelszót és toonot beállítva hello jelszó lejár.

* Az **nem támogatott** toochange vagy alaphelyzetbe állítása hello jelszavát hello szolgáltatást. Így megsemmisít hello titkosítási kulcsok és hello szolgáltatás nem tud tooaccess hello adatbázis és nem tudja toostart.

## <a name="changes-toohello-scheduler"></a>Módosítások toohello Feladatütemező
Hello kiadásokban az 1.1-es build kezdve (2016. februári) is beállíthat hello [Feladatütemező](active-directory-aadconnectsync-feature-scheduler.md) toohave más szinkronizálási ciklust hello alapértelmezett 30 perc.

## <a name="changes-toosynchronization-rules"></a>Módosítások tooSynchronization szabályok
hello telepítővarázslója biztosít a konfigurációkat, amelyek a leggyakoribb esetekben hello toowork kellene. Abban az esetben toomake módosítások toohello konfigurációs van szüksége, majd el kell végeznie ezeket a szabályokat toostill támogatott konfigurációval rendelkezik.

* Is [attribútumfolyamok módosítása](active-directory-aadconnectsync-change-the-configuration.md#other-common-attribute-flow-changes) Ha hello alapértelmezett közvetlen attribútumfolyamok nem alkalmasak a szervezet számára.
* Ha azt szeretné, túl[attribútum nem flow](active-directory-aadconnectsync-change-the-configuration.md#do-not-flow-an-attribute) , és távolítsa el valamelyik meglévő attribútum értékei az Azure AD-toocreate szabály ehhez a forgatókönyvhöz kell használnia.
* [Egy nem kívánt szinkronizálási szabály letiltása](#disable-an-unwanted-sync-rule) ahelyett, hogy törölné. A törölt szabály jön létre újra a frissítés során.
* túl[egy out-of-box szabály módosítása](#change-an-out-of-box-rule), ügyeljen másolatát hello eredeti szabályt, és tiltsa le a hello out-of-box szabály. Szinkronizálási szabály szerkesztő hello kér, és a segítségével.
* Exportálja az egyéni szinkronizálási szabályok hello szinkronizálási szabályok szerkesztő használatával. hello szerkesztő lehetővé teszi egy PowerShell-parancsfájlt használhatja a tooeasily hozza létre őket katasztrófa utáni helyreállítás esetén.

> [!WARNING]
> hello out-of-box szinkronizálási szabályok rendelkeznek egy ujjlenyomatot. Módosítja toothese szabályokat, ha a rendszer már nem megfelelő hello ujjlenyomata. A jövőbeli hello problémái lehetnek az új verziót az Azure AD Connect tooapply meg. Csak utat módosítások hello leírása az ebben a cikkben.

### <a name="disable-an-unwanted-sync-rule"></a>Egy nem kívánt szinkronizálási szabály letiltása
Ne törölje az out-of-box szinkronizálási szabály. Az következő frissítés során újból létrejön.

Bizonyos esetekben hello telepítési varázsló által előállított a konfigurációkat, amelyek a topológia nem működik. Például ha egy fiók-erőforrás szolgának rendelkezik, de a kiterjesztett hello séma hello fiókerdő hello Exchange sémával rendelkezik, majd az Exchange szabályok létrehozása a hello fiókerdő és hello erőforráserdőben. Ebben az esetben szüksége toodisable hello szinkronizálási szabály Exchange-hez.

![Letiltott szinkronizálási szabály](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/exchangedisabledrule.png)

A fenti hello kép hello telepítővarázsló talált egy régi Exchange 2003 séma hello fiókerdő. A séma kiterjesztése előtt hello Erőforráserdő Fabrikam környezetben jelent meg. tooensure hello régi Exchange megvalósítása nem attribútumok szinkronizálva, hello szinkronizálási szabályt le kell tiltani látható módon.

### <a name="change-an-out-of-box-rule"></a>Az out-of-box szabály módosítása
hello csak meg kell változtatni egy out-of-box szabály, amikor toochange hello illesztési szabály van szüksége. Ha egy Attribútumfolyam toochange van szüksége, majd kell szabályt hoz létre egy szinkronizálási hello out-of-box szabályoktól magasabb prioritással. egyetlen szabály tooclone gyakorlatilag kell az hello szabály hello **a az AD - felhasználó csatlakozás**. Minden más szabályaival magasabb elsőbbséget szabályok felülbírálásához.

Ha toomake módosítások tooan out-of-box szabály van szüksége, majd meg kell készítsen másolatot hello out-of-box szabály és hello eredeti szabálynak a letiltása. Végezze el hello módosítások toohello klónozott szabály. Szinkronizálási szabály szerkesztő hello elősegíti az ezeket a lépéseket. Amikor megnyit egy out-of-box szabály, lehetősége lesz ezen a párbeszédpanelen:  
![Figyelmeztetés kívül mezőben szabály](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Válassza ki **Igen** toocreate hello szabály egy példányát. hello klónozott szabály nyitja meg.  
![Klónozott szabály](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/clonedrule.png)

Győződjön meg a klónozott szabály, a szükséges módosításokat tooscope, csatlakozási és átalakítások.

## <a name="next-steps"></a>Következő lépések
**Áttekintő témakör**

* [Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
