---
title: "aaaTroubleshoot StorSimple telepítésekkel kapcsolatos problémákhoz |} Microsoft Docs"
description: "Ismerteti, hogyan toodiagnose és javítsa ki a hibákat, fordulhat elő, amikor először telepíti a StorSimple."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: f8352eaa-193c-42d1-8818-0b8e02d8485d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/18/2016
ms.author: alkohli
ms.openlocfilehash: 9a31a3cfb3be577500b226c2bc8172dd8664bad9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-storsimple-device-deployment-issues"></a>A StorSimple eszköz telepítési problémák elhárításához
## <a name="overview"></a>Áttekintés
Ez a cikk hasznos elhárításához nyújt útmutatást a Microsoft Azure StorSimple üzembe helyezésére. Ismerteti a gyakori problémákat, a lehetséges okok és a javasolt lépéseket toohelp StorSimple konfigurálásakor az esetleg előforduló problémák megoldása. Ezek az információk tooboth hello StorSimple helyszíni fizikai eszköz és a StorSimple virtuális eszköz hello vonatkoznak.

> [!NOTE]
> Eszköz konfigurációs kapcsolatos problémák, előfordulhat, hogy szembesülhetnek akkor fordulhat elő, amikor hello hello az eszközön első alkalommal telepíteni, vagy később, akkor fordulhat működési hello eszköz esetén. Ez a cikk foglalkozik az első központi telepítési problémák elhárításához. túl nyissa meg az operatív eszköz tootroubleshoot[működési eszköz elhárítása](storsimple-troubleshoot-operational-device.md).
> 
> 

Ez a cikk is ismerteti a StorSimple központi telepítések hibaelhárítási hello eszközök, és a részletes hibaelhárítási példa.

## <a name="first-time-deployment-issues"></a>Első telepítésekkel kapcsolatos problémákhoz
Ha futtatja a hibát először az eszközhöz tartozó hello telepítésekor, vegye figyelembe a hello következőket:

* Ha egy fizikai eszköz hibaelhárítást, ellenőrizze, hogy hello hardver telepítve és leírtak szerint konfigurálta [a StorSimple 8100 eszköz telepítése](storsimple-8100-hardware-installation.md) vagy [a StorSimple 8600 eszköztelepítése](storsimple-8600-hardware-installation.md).
* Ellenőrizze a telepítés előfeltételeit. Győződjön meg arról, hogy rendelkezik-e hello leírt összes hello adatokat [üzembehelyezési konfigurációs ellenőrzőlista](storsimple-deployment-walkthrough.md#deployment-configuration-checklist).
* Tekintse át a hello StorSimple Release Notes toosee, ha hello a problémát. hello kibocsátási megjegyzések a lehetséges megoldások a ismert telepítési problémákat. 

Eszköz telepítése során a leggyakoribb hello arcfelismerési fordulhat elő, hogy a felhasználók problémákat a hello beállítása varázsló futtatásakor, és amikor regisztrálják az hello eszköz Windows PowerShell segítségével a StorSimple. (Használjon Windows PowerShell StorSimple tooregister, és konfigurálhatja a StorSimple eszközt. Az eszközregisztráció további információkért lásd: [3. lépés: konfigurálása és a Windows PowerShell segítségével az eszköz regisztrálása a StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)).

a következő szakaszok hello az első alkalommal hello hello StorSimple eszköz konfigurálásakor előforduló problémák megoldásához nyújt segítséget.

## <a name="first-time-setup-wizard-process"></a>Először a telepítő varázsló folyamat
a lépéseket követve hello hello beállítása varázsló folyamat foglalják össze. A telepítő részletes információkért lásd: [a helyszíni StorSimple eszköz üzembe helyezése](storsimple-deployment-walkthrough.md).

1. Futtassa a hello [Invoke-HcsSetupWizard](https://technet.microsoft.com/library/dn688135.aspx) parancsmag toostart hello beállítása varázsló, amely végigvezeti Önt hello hátralévő lépéseket. 
2. Hálózati hello konfigurálása: hello telepítő varázsló lehetővé teszi a StorSimple eszköz hello DATA 0 hálózati adapterén hálózati beállításainak konfigurálása. Ezek a beállítások hello következőket tartalmazzák:
   * Virtuális IP-cím (VIP), alhálózati maszk és átjáró – hello [Set-HcsNetInterface](https://technet.microsoft.com/library/dn688161.aspx) parancsmag végrehajtása hello háttérben. A StorSimple eszköz beállítja hello IP-cím, alhálózati maszk és átjáró hello DATA 0 hálózati adapterén.
   * Elsődleges DNS-kiszolgáló – hello [Set-HcsDnsClientServerAddress](https://technet.microsoft.com/library/dn688172.aspx) parancsmag végrehajtása hello háttérben. Beállítja a StorSimple megoldásban hello DNS-beállításait.
   * NTP-kiszolgáló – hello [Set-HcsNtpClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) parancsmag végrehajtása hello háttérben. Beállítja hello NTP-kiszolgáló beállításai a StorSimple megoldásban.
   * Nem kötelező webalkalmazás-proxy – hello [Set-HcsWebProxy](https://technet.microsoft.com/library/dn688154.aspx) parancsmag végrehajtása hello háttérben. Beállítja, és lehetővé teszi, hogy a hello webproxy konfigurálása a StorSimple megoldásban.
3. Hello jelszavak beállítása: hello következő lépésre tooset eszköz-rendszergazdai és a StorSimple Snapshot Manager jelszavát. Ha futtatja az 1. frissítést, majd csak akkor szükséges tooset be hello StorSimple Snapshot Manager jelszavát.
   
   * hello eszköz rendszergazdai jelszava használt toolog tooyour eszközön. hello eszköz alapértelmezett jelszava: **jelszó1**.
   * hello StorSimple Snapshot Manager jelszavának konfigurálása a StorSimple Snapshot Manager eszköz toouse szükség. Toofirst hello jelszó beállítása hello telepítővarázsló van szüksége, és ezután állítsa be, és módosítsa úgy a StorSimple Manager szolgáltatás hello. Ez a jelszó hello eszköz StorSimple Snapshot Manager hitelesíti.
     
     > [!IMPORTANT]
     > Jelszavak bejegyzése előtt gyűjtött, de csak a sikeresen hello eszköz regisztrálása után alkalmazza. Ha a hiba tooapply a jelszó, fogja felszólító toosupply hello jelszó újra hello kötelező, a rendszer a jelszavak (hello összetettségi követelményeknek) gyűjti.
     > 
     > 
4. Hello-eszköz regisztrálása: hello utolsó lépés egy tooregister hello eszköz a Microsoft Azure-beli hello StorSimple Manager szolgáltatásban. hello regisztrációs túl szükséges[hello Szolgáltatásregisztrációs kulcs lekérése](storsimple-manage-service.md#get-the-service-registration-key) a hello a klasszikus Azure portálon, és hello beállítása varázslóban adja meg. Miután hello eszköz regisztrálása sikeres volt, akkor a szolgáltatásadat-titkosítási kulcs tooyou valósul meg. Győződjön meg arról, hogy a titkosítási kulcs egy biztonságos helyre, mert lesz tookeep szükséges tooregister hello szolgáltatást minden ezt követő eszközt.

## <a name="common-errors-during-device-deployment"></a>Eszköz telepítése során előforduló hibákat
hello táblák listáját hello közös észlelt hibák, hogy előfordulhat, hogy mikor követően, hogy:

* Hello szükséges hálózati beállításainak konfigurálása.
* Hello választható webes proxy beállításainak konfigurálása.
* Hello eszköz-rendszergazdai és a StorSimple Snapshot Manager jelszavak beállítása. 
* Hello eszközt regisztrálni kell. 

## <a name="errors-during-hello-required-network-settings"></a>Hibák hello során szükséges hálózati beállításai
| Nem. | Hibaüzenet | Lehetséges okok | Javasolt művelet |
| --- | --- | --- | --- |
| 1 |Invoke-HcsSetupWizard: Ez a parancs csak futtatható hello aktív vezérlő. |Konfigurációs hello passzív vezérlő végrehajtása. |Futtassa a parancsot hello aktív vezérlő. További információkért lásd: [azonosíthatja az aktív vezérlőhöz az eszközön](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device). |
| 2 |Invoke-HcsSetupWizard: Az eszköz nem üzemkész. |A DATA 0 hello hálózati kapcsolattal rendelkező problémák vannak. |Ellenőrizze a DATA 0 hello fizikai hálózati kapcsolatot. |
| 3 |Invoke-HcsSetupWizard: Nincs más rendszerrel hello hálózat IP-címütközést (kivétel HRESULT: 0x80070263). |DATA 0 megadott hello IP már egy másik rendszer használatban volt. |Adjon meg egy új IP-cím nincs használatban. |
| 4 |Invoke-HcsSetupWizard: A fürterőforrás nem sikerült. (Kivétel HRESULT: 0x800713AE). |Ismétlődő VIP. a megadott hello IP már használatban van. |Adjon meg egy új IP-cím nincs használatban. |
| 5 |Invoke-HcsSetupWizard: Érvénytelen IPv4-címet. |hello IP-cím formátuma nem megfelelő valósul meg. |Ellenőrizze a hello formátumú, és adja meg újra az IP-címe. További információkért lásd: [Ipv4-címzési][1]. |
| 6 |Invoke-HcsSetupWizard: Érvénytelen IPv6-címet. |hello IP-cím formátuma nem megfelelő valósul meg. |Ellenőrizze a hello formátumú, és adja meg újra az IP-címe. További információkért lásd: [IPv6-címzés][2]. |
| 7 |Invoke-HcsSetupWizard: Hello végpontleképező rendelkezésre állnak olyan további végpontok. (Kivétel HRESULT: 0x800706D9) |hello fürt nem működőképes. |[Forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) további lépéseket. |

## <a name="errors-during-hello-optional-web-proxy-settings"></a>Hibák során hello választható webproxy beállításai
| Nem. | Hibaüzenet | Lehetséges okok | Javasolt művelet |
| --- | --- | --- | --- |
| 1 |Invoke-HcsSetupWizard: Érvénytelen paraméter (kivétel HRESULT: 0x80070057) |Hello proxybeállítások megadott hello paraméterek egyike nem érvényes. |hello URI azonosítója nincs megadva hello megfelelő formátumban. A következő formátumot használja hello: http://*<IP address or FQDN of hello web proxy server>*:*<TCP port number>* |
| 2 |Invoke-HcsSetupWizard: RPC-kiszolgáló nem érhető el (kivétel HRESULT: 0x800706BA jelű) |hello okozza-e hello a következők egyikét:<ol><li>hello fürt nem működik-e.</li><li>hello passzív vezérlő hello aktív vezérlő nem tud kommunikálni, és hello parancsot futtatja a passzív vezérlőről.</li></ol> |Attól függően, hogy hello alapvető ok:<ol><li>[Forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) toomake meg arról, hogy hello fürt működik-e.</li><li>Hello parancsot a hello aktív vezérlő. Ha azt szeretné, hogy toorun hello parancs hello passzív vezérlőből, szüksége lesz a tooensure adott hello passzív vezérlő kommunikálhatnak hello aktív vezérlő. Szüksége lesz túl[forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) Ha a kapcsolat megszakad.</li></ol> |
| 3 |Invoke-HcsSetupWizard: RPC-hívása sikertelen volt (kivétel HRESULT: 0x800706be) |Fürt nem működik. |[Forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) toomake meg arról, hogy hello fürt működik-e. |
| 4 |Invoke-HcsSetupWizard: Fürt erőforrás nem található (kivétel HRESULT: 0x8007138f) |hello fürt erőforrás nem található. Ez akkor fordulhat elő, amikor hello telepítés nem volt megfelelő. |Szükség lehet tooreset hello eszköz toohello gyári beállításait. [Forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) toocreate fürt erőforrás. |
| 5 |Invoke-HcsSetupWizard: Fürt erőforrás nincs online állapotban (kivétel HRESULT: 0x8007138c) |Fürt erőforrás nincs online állapotban. |[Forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) további lépéseket. |

## <a name="errors-related-toodevice-administrator-and-storsimple-snapshot-manager-passwords"></a>Toodevice rendszergazda és a StorSimple Snapshot Manager jelszavak kapcsolatos hibák
hello alapértelmezett eszköz rendszergazdai jelszava van **jelszó1**. A jelszó lejárati követő hello első bejelentkezéskor; ezért toouse hello beállítása varázsló toochange kell azt. Amikor regisztrálja a hello hello az eszközön első alkalommal meg kell adnia egy új eszköz rendszergazdai jelszava. 

Hello StorSimple Snapshot Manager szoftver eszközön futó hello Windows Server állomás toomanage hello használata, majd meg kell adni a StorSimple Snapshot Manager jelszava első regisztráció során. 

Győződjön meg arról, hogy a jelszavak megfelelnek-e a követelményeknek hello:

* Az eszköz rendszergazdai jelszavának 8 – 15 karakter között kell lennie.
* A StorSimple Snapshot Manager jelszavát 14 vagy 15 karakter hosszú lehet.
* Jelszavak tartalmaznia kell a 3. a következő 4 karaktertípusokat hello:, kisbetű, nagybetű, numerikus és speciális. 
* A jelszó azonos a legutóbbi 24 jelszó hello nem hello.

Továbbá ne feledje, hogy a jelszavak lejárnak évente, és csak a sikeresen hello eszköz regisztrálása után módosíthatja. Hello regisztráció bármilyen okból nem sikerül, ha hello jelszavakat nem lehet módosítani. További információ az eszköz-rendszergazdai és a StorSimple Snapshot Manager jelszavát: túl[használata hello StorSimple Manager szolgáltatás toochange a StorSimple-jelszavak](storsimple-change-passwords.md).

Egy vagy több hello hello eszköz-rendszergazdai és a StorSimple Snapshot Manager jelszavak beállítása során a következő hibák fordulhatnak elő.

| Nem. | Hibaüzenet | Javasolt művelet |
| --- | --- | --- |
| 1 |hello jelszó hossza meghaladja a hello maximális hosszát. |Használjon olyan jelszót, amely megfelel ezeknek a követelményeknek:<ul><li>Az eszköz rendszergazdai jelszavának 8 – 15 karakter között kell lennie.</li><li>A StorSimple Snapshot Manager jelszavát 14 vagy 15 karakter hosszúságúnak kell lennie.</li></ul> |
| 2 |hello jelszó nem felel meg a szükséges hello hossza. |Használjon olyan jelszót, amely megfelel ezeknek a követelményeknek:<ul><li>Az eszköz rendszergazdai jelszavának 8 – 15 karakter között kell lennie.</li><li>A StorSimple Snapshot Manager jelszavát 14 vagy 15 karakter hosszúságúnak kell lennie.</lu></ul> |
| 3 |hello jelszó kisbetűs karaktert kell tartalmaznia. |Jelszavak tartalmaznia kell a 3. a következő 4 karaktertípusokat hello:, kisbetű, nagybetű, numerikus és speciális. Győződjön meg arról, hogy a jelszó megfelel-e ezek a követelmények. |
| 4 |hello jelszónak számokat kell tartalmaznia. |Jelszavak tartalmaznia kell a 3. a következő 4 karaktertípusokat hello:, kisbetű, nagybetű, numerikus és speciális. Győződjön meg arról, hogy a jelszó megfelel-e ezek a követelmények. |
| 5 |hello jelszó különleges karaktereket tartalmazhat. |Jelszavak tartalmaznia kell a 3. a következő 4 karaktertípusokat hello:, kisbetű, nagybetű, numerikus és speciális. Győződjön meg arról, hogy a jelszó megfelel-e ezek a követelmények. |
| 6 |hello jelszónak tartalmaznia kell 4 karakter típus a következő hello 3: nagybetűk, a kis, a numerikus és a speciális. |A jelszó nem tartalmaz karaktert szükséges hello típusú. Győződjön meg arról, hogy a jelszó megfelel-e ezek a követelmények. |
| 7 |A paraméter nem felel meg a megerősítő. |Győződjön meg arról, hogy a jelszó megfelel-e az összes követelményeknek, és hogy helyesen írta be. |
| 8 |A jelszó nem egyezhet meg a hello alapértelmezett. |hello alapértelmezett jelszó *jelszó1*. Szüksége toochange ezt a jelszót a bejelentkezés után a hello első alkalommal. |
| 9 |hello a megadott jelszó nem egyezik meg a hello eszköz jelszavát. Írja be újra hello jelszót. |Ellenőrizze hello jelszavát, és írja be újra. |

Jelszavak összegyűjtése előtt hello eszköz regisztrálva van, de csak a sikeres regisztrációt követően lesznek alkalmazva. hello jelszó helyreállítási munkafolyamat hello eszköz toobe regisztrálva van szükség. 

> [!IMPORTANT]
> Általánosságban elmondható Ha egy kísérlet tooapply jelszó sikertelen lesz, majd hello szoftver ismételten megkísérli toocollect hello jelszó, amíg az sikeres nem. Ritka esetekben hello jelszó nem lehet alkalmazni. Ebben a helyzetben hello eszköz regisztrációját, és a folytatáshoz azonban hello jelszavak nem fognak megváltozni. Kapni fog nem jelzi, toowhich jelszava nem módosult – hello eszköz rendszergazdai jelszava vagy hello StorSimple Snapshot Manager jelszavát. Ez a helyzet akkor fordul elő, ha azt javasoljuk, hogy módosítsa a mindkét jelszóval.
> 
> 

A klasszikus Azure portálon keresztül hello StorSimple Manager szolgáltatás hello hello jelszavak alaphelyzetbe állíthatja. További információkért lépjen: 

* [Változás hello eszköz rendszergazdai jelszava](storsimple-change-passwords.md#change-the-device-administrator-password).
* [StorSimple Snapshot Manager jelszavának módosítása hello](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

## <a name="errors-during-device-registration"></a>Eszközök regisztrációja során hibák
Hello StorSimple Manager szolgáltatás fut a Microsoft Azure tooregister hello eszközt használja. Egy vagy több problémák eszközök regisztrációja során a következő hello sikerült tapasztal.

| Nem. | Hibaüzenet | Lehetséges okok | Javasolt művelet |
| --- | --- | --- | --- |
| 1 |350027. hiba: Nem sikerült a StorSimple Manager hello tooregister hello eszköz. | |Várjon néhány percet, és próbálkozzon újra a művelettel hello. Ha hello a probléma továbbra is fennáll, [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md). |
| 2 |350013. hiba: Hiba történt a hello eszköz regisztrálása. Ennek oka lehet tooincorrect Szolgáltatásregisztrációs kulcs. | |Regisztrálja hello eszköz újra hello megfelelő szolgáltatás regisztrációs kulcsával. További információkért lásd: [hello Szolgáltatásregisztrációs kulcs lekérése.](storsimple-manage-service.md#get-the-service-registration-key) |
| 3 |350063. hiba: Hitelesítési tooStorSimple kezelő szolgáltatás kapott, de nem sikerült regisztrálni. Hello műveletet egy kis idő múlva próbálkozzon újra. |Ez a hiba jelzi, hogy a hitelesítést az ACS használatával a rendszer megfelelt, de hello register híváshoz toohello szolgáltatásnak nem sikerült. Ennek oka lehet egy szórványos hálózati működési hiba eredménye. |Ha hello a probléma továbbra is fennáll, adjon [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md). |
| 4 |350049. hiba: hello szolgáltatás nem érhető el regisztrálás során. |Toohello szolgáltatás hello telefonhívást indít, amikor egy webes kivétel érkezett. Néhány esetben ez lehetséges, hogy első rögzített hello művelet később. |Ellenőrizze az IP-cím és DNS-nevét, majd próbálkozzon újra a hello műveletet. Ha hello a probléma továbbra is fennáll, [forduljon a Microsoft Support.](storsimple-contact-microsoft-support.md) |
| 5 |350031. hiba: hello eszköz már regisztrálva van. | |Nincs szükség műveletre. |
| 6 |350016. hiba: Nem sikerült regisztrálni. | |Ellenőrizze, hogy hello regisztrációs kulcs helyességéről. |
| 7 |Invoke-HcsSetupWizard: Hiba történt az eszköz; regisztrálása során Ennek oka lehet tooincorrect IP-címe vagy DNS-nevét. A hálózati beállításokat, és próbálkozzon újra. Ha hello a probléma továbbra is fennáll, [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md). (350050 hiba) |Győződjön meg arról, hogy az eszköz pingelése hello hálózaton kívülről. Ha nincs kapcsolat toooutside hálózati, a hello regisztráció sikertelen lehet, és ezt a hibát. Ez a hiba legalább egy hello következő kombinációja lehet:<ul><li>Helytelen IP</li><li>Helytelen alhálózati</li><li>Helytelen átjáró</li><li>Helytelen DNS-beállítások</li></ul> |Tekintse meg a szükséges lépések hello hello [részletes hibaelhárítási példa](#step-by-step-storsimple-troubleshooting-example). |
| 8 |Invoke-HcsSetupWizard: hello aktuális művelet tooan belső szolgáltatási hiba [0x1FBE2] miatt nem sikerült. Hello művelet némi várakozás után próbálkozzon újra. Ha hello a probléma továbbra is fennáll, forduljon a Microsoft Support. |Ez egy általános hiba lépett fel az összes felhasználó nem látható hiba a szolgáltatás vagy az ügynök. hello Ennek leggyakoribb oka lehet, hogy az ACS hello a hitelesítés sikertelen volt. A lehetséges hello hiba oka, hogy hello NTP-kiszolgáló konfigurációs problémák és hello eszköz ideje nem megfelelően van beállítva. |Javítsa ki a hello idő (Ha problémák vannak), majd próbálkozzon újra a hello regisztrációs műveletet. Hello Set-HcsSystem - időzóna parancs tooadjust hello időzóna használatakor szókezdő hello időzónában (például "csendes-óceáni téli idő").  Ha a probléma tartósan fennáll, [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) további lépéseket. |
| 9 |Figyelmeztetés: Nem sikerült hello eszköz aktiválásához. Az eszköz-rendszergazdai és a StorSimple Snapshot Manager jelszavát nem módosult. |Hello regisztrálása meghiúsul, ha a hello eszköz-rendszergazdai és a StorSimple Snapshot Manager jelszavát a, nem módosulnak. | |

## <a name="tools-for-troubleshooting-storsimple-deployments"></a>StorSimple központi telepítések hibaelhárítási eszközök
StorSimple használható tootroubleshoot a StorSimple megoldásban több eszközt tartalmaz. Ezek a következők:

* Támogatási csomag és eszköznaplók 
* A hibaelhárításhoz kifejezetten parancsmagok 

## <a name="support-packages-and-device-logs-available-for-troubleshooting"></a>Csomagok és a rendelkezésre álló eszköznaplók hibaelhárítás támogatása
Egy támogatási csomag tartalmazza, melyek segíthetik a hibaelhárításban eszközökkel kapcsolatos problémákat a hello Microsoft Support csoport összes hello megfelelő napló. A Windows PowerShell használható StorSimple toogenerate egy titkosított támogatási csomagot, majd megosztása a technikai tanácsadási csoporthoz.

### <a name="tooview-hello-logs-or-hello-contents-of-hello-support-package"></a>tooview hello naplók vagy hello hello tartalmát támogatási csomag
1. Windows PowerShell használata a StorSimple toogenerate egy támogatási csomag leírtak [létrehozása és kezelése egy támogatási csomag](storsimple-create-manage-support-package.md).
2. Töltse le a hello [visszafejtési parancsfájl](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) helyileg az ügyfélszámítógépen.
3. Ezzel [lépésről lépésre](storsimple-create-manage-support-package.md#edit-a-support-package) tooopen és visszafejtése hello támogatási csomag.
4. hello támogatási csomag etw/etvx formátumban vannak naplók visszafejtése. A Windows eseménynaplójában a következő lépéseket tooview hello ezeket a fájlokat hajthatja végre:
   
   1. Futtassa a hello **eventvwr** parancsot a Windows-ügyfeleken. Ekkor elindul az Eseménynapló hello.
   2. A hello **műveletek** ablaktáblán kattintson a **naplófájl megnyitása** és naplófájlok pont toohello etvx/etw-formátumban (hello támogatási csomag). Most már megtekintheti a hello fájlt. Hello fájl megnyitása után kattintson a jobb gombbal, és hello fájl mentése szövegként.
      
      > [!IMPORTANT]
      > Is használhatja a hello **Get-WinEvent** parancsmag tooopen ezeket a fájlokat, a Windows PowerShellben. További információkért lásd: [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx) a hello Windows PowerShell parancsmag ismertető dokumentációjában.
      > 
      > 
5. Megnyitásakor hello naplók az eseménynaplóban, keresse meg a következő problémák kapcsolódó toohello eszközök konfigurációját tartalmazó naplófájlok hello:
   
   * hcs_pfconfig/műveleti napló
   * hcs_pfconfig/Config
6. A hello naplófájlokat keresse meg a karakterláncok hello beállítása varázsló által meghívott kapcsolódó toohello parancsmagok. Lásd: [először a telepítő varázsló folyamat](#first-time-setup-wizard-process) ezek a parancsmagok listáját. 
7. Ha nem tudja toofigure hello probléma okát hello ki, akkor [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) további lépéseket. Használjon hello szükséges lépések [hozzon létre egy támogatási kérést](storsimple-contact-microsoft-support.md#create-a-support-request) amikor Microsoft Support szolgálatához segítségért.

## <a name="cmdlets-available-for-troubleshooting"></a>A hibaelhárításhoz elérhető parancsmagok
A következő Windows PowerShell parancsmagok toodetect csatlakozási hibák hello használata.

* `Get-NetAdapter`: Ez a parancsmag toodetect hello állapotát, a hálózati adapterek használata. 
* `Test-Connection`: Ez a parancsmag toocheck hello hálózati kapcsolat hello hálózati kívül és belül használja.
* `Test-HcsmConnection`: Ez a parancsmag toocheck hello kapcsolatát sikeresen regisztrált egy eszközt használja.

Ha a StorSimple eszköz frissítése 1 futtatja, a következő diagnosztikai parancsmagok hello is elérhetők.

* `Sync-HcsTime`: Ez a parancsmag toodisplay eszköz alkalommal használniuk, és időszinkronizálást hello NTP-kiszolgálóval.
* `Enable-HcsPing`és `Disable-HcsPing`: a StorSimple eszköz ezen parancsmagok tooallow hello állomások tooping hello hálózati illesztőket használhat. Alapértelmezés szerint hello StorSimple hálózati adapterek nem válaszolnak tooping kérelmeket.
* `Trace-HcsRoute`: Ez a parancsmag használata olyan útvonal eszköz. Hello módon tooa végső rendeltetési tooeach útválasztó csomagokat küld egy meghatározott időtartamra vonatkozóan, és majd kiszámítja a hello csomagok egyes ugrások által visszaadott eredmény. Mivel `Trace-HcsRoute` hello csomagvesztés bármely adott útválasztó vagy a hivatkozás, bizonyos fokú jeleníti meg, mely útválasztók tudja, vagy előfordulhat, hogy a hivatkozások hálózati problémát okoz. 
* `Get-HcsRoutingTable`: Ez a parancsmag toodisplay hello helyi IP-útválasztási táblázat használja.

## <a name="troubleshoot-with-hello-get-netadapter-cmdlet"></a>Hello Get-NetAdapter parancsmaggal hibaelhárításához
Hálózati illesztők első eszköz üzembe helyezés konfigurálásakor hello hardverállapot nem áll rendelkezésre a StorSimple Manager szolgáltatás felhasználói felületi hello mivel hello eszköz még nincs regisztrálva a hello szolgáltatást. Emellett hello hardver állapotlapon előfordulhat, hogy nem mindig tükrözi megfelelően hello állapotának hello eszköz, különösen akkor, ha a szinkronizálási szolgáltatást érintő problémák vannak. Ezekben a helyzetekben használhatja hello `Get-NetAdapter` parancsmag toodetermine hello a hálózati adapterek állapotát.

### <a name="toosee-a-list-of-all-hello-network-adapters-on-your-device"></a>az eszköz összes hello hálózati adapterek listájának toosee
1. Indítsa el a Windows PowerShell a StorSimple, és írja be `Get-NetAdapter`. 
2. Hello kimenete hello használata `Get-NetAdapter` parancsmag és a következő irányelveket toounderstand hello hello az a hálózati illesztő állapotát.
   
   * Ha hello kapcsolat kifogástalan, és engedélyezve van, hello **ifIndex** állapot jelenik meg **be**.
   * Ha hello felület működik megfelelően, de nem fizikailag kapcsolódás (hálózati kábel), hello **ifIndex** jelenik meg, mint **letiltott**.
   * Ha hello illesztő működik megfelelően, de nincs engedélyezve, hello **ifIndex** állapot jelenik meg **NotPresent**.
   * Ha hello felület nem létezik, nem jelenik meg ebben a listában. StorSimple Manager szolgáltatás felhasználói felületi hello továbbra is megjelenik ez az interfész hibás állapotban.

További információt a toouse Ez a parancsmag lépjen túl[GetNetAdapter](https://technet.microsoft.com/library/jj130867.aspx) hello Windows PowerShell parancsmag-referencia a. 

hello következő szakaszok bemutatják hello kimenetét minták `Get-NetAdapter` parancsmag. 

 Ezeket a mintákat vezérlő 0 hello passzív vezérlő lett, és a következőképpen volt konfigurálva:

* DATA 0, az adatok 1, a DATA 2 és a DATA 3 hálózati adapterek már létezett a hello eszközön.
* ADATOK 4 és az adatok 5 hálózati kártyák nem volt jelen; ezért nem szerepelnek hello kimenet.
* DATA 0 volt engedélyezve.

1. vezérlő hello aktív vezérlő volt, és a következőképpen volt konfigurálva:

* DATA 0, adatok 1, 2 adatok, a DATA 3, adatok 4 és a DATA 5 hálózati adapterek már létezett a hello eszközön.
* DATA 0 volt engedélyezve.

**Példa a kimenetre – vezérlő 0**

hello az alábbiakban az hello kimenetét a vezérlő 0 (hello passzív tartományvezérlő). ADATOK 1, a DATA 2 és a DATA 3 nincs csatlakoztatva. ADATOK 4 és az adatok 5 nem tartalmazzák, mivel nincsenek hello eszközön jelenlévő. 

     Controller0>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter #2     17       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter        14       NotPresent
     Ethernet 2           HCS VNIC                                    13       Up
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up


**Példa a kimenetre – 1. vezérlő**

hello hello kimeneti vezérlőből (aktív vezérlő hello) 1 látható. Csak hello DATA 0 hálózati csatoló hello eszközön van konfigurálva, és működik.

     Controller1>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter        18       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter #2     19       NotPresent
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up
     Ethernet 2           HCS VNIC                                    13       Up
     DATA5                Intel(R) Gigabit ET Dual Port Server...     14       NotPresent
     DATA4                Intel(R) Gigabit ET Dual Port Serv...#2     17       NotPresent


## <a name="troubleshoot-with-hello-test-connection-cmdlet"></a>Hello Test-Connection parancsmaggal hibaelhárításához
Használhatja a hello `Test-Connection` parancsmag toodetermine e a StorSimple eszköz csatlakozni tud-toohello hálózaton kívülről. Ha minden hello hálózati paramétert, beleértve a DNS, hello hello telepítővarázsló megfelelően vannak konfigurálva, használhatja a hello `Test-Connection` parancsmag tooping hello hálózaton, például az Outlook.com-on kívüli ismert címnek. 

Ping tootroubleshoot csatlakozási hibák léptek fel ezt a parancsmagot akkor engedélyezze, ha ping le van tiltva.

Lásd: a minta kimenet követően – hello hello `Test-Connection` parancsmag. 

> [!NOTE]
> Hello első minta hello eszköz egy helytelen DNS van beállítva. Hello második példában hello DNS helyességéről.
> 
> 

**Példa a kimenetre – helytelen DNS**

A következő minta hello nincs nincs kimenet hello IPV4 és IPv6 típusú címek, amely azt jelzi, hogy nincs feloldva a DNS hello. Ez azt jelenti, hogy nincs kapcsolat toohello hálózaton kívül van, a megfelelő DNS megadott toobe. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com

**Példa a kimenetre – megfelelő DNS**

A következő minta hello hello DNS értéket ad vissza hello IPv4-cím, jelezve, hogy hello DNS megfelelően van konfigurálva. Ez megerősíti, hogy van-e kapcsolat toohello hálózaton kívülről. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194

## <a name="troubleshoot-with-hello-test-hcsmconnection-cmdlet"></a>Hibaelhárítás a hello Test-HcsmConnection parancsmag
Használjon hello `Test-HcsmConnection` parancsmag egy eszköz már csatlakoztatva tooand regisztrálni a StorSimple Manager szolgáltatással. Ez a parancsmag segítségével egy regisztrált eszköz és a StorSimple Manager szolgáltatás megfelelő hello közötti hello kapcsolat ellenőrzése. Ez a parancs a StorSimple futtathatja a Windows PowerShell. 

### <a name="toorun-hello-test-hcsmconnection-cmdlet"></a>toorun hello Test-HcsmConnection parancsmag
1. Győződjön meg arról, hogy hello eszköz regisztrálva van.
2. Ellenőrizze a hello eszköz állapotát. Ha hello eszközt az Inaktiválás karbantartási módban, vagy offline módban vannak, a következő hibák hello merülhetnek fel: 
   
   * ErrorCode.CiSDeviceDecommissioned – Ez azt jelzi, hogy hello eszközt az Inaktiválás.
   * ErrorCode.DeviceNotReady – Ez azt jelzi, hogy hello eszköz karbantartási módban van.
   * ErrorCode.DeviceNotReady – az azt jelenti, hogy hello eszköz nem online.
3. Győződjön meg arról, hogy hello StorSimple Manager szolgáltatás fut. (hello használata [Get-ClusterResource](https://technet.microsoft.com/library/ee461004.aspx) parancsmag). Ha hello szolgáltatás nem fut, a következő hibák hello merülhetnek fel:
   
   * ErrorCode.CiSApplianceAgentNotOnline
   * ErrorCode.CisPowershellScriptHcsError – Ez azt jelzi, hogy kivételt Get-ClusterResource futtatásakor.
4. Ellenőrizze a hello Access Control Service (ACS) jogkivonatot. Ha azt a webes kivételt okoz, lehet, hello átjáró probléma, egy hiányzó proxyhitelesítés, egy helytelen DNS vagy hitelesítési hiba eredménye. A következő hibák hello tapasztalhatja:
   
   * ErrorCode.CiSApplianceGateway – Ez jelzi a HttpStatusCode.BadGateway kivétel: hello feloldó szolgáltatás nem tudta feloldani a hello állomásnevet. 
   * ErrorCode.CiSApplianceProxy – Ez jelzi (HTTP-állapotkód 407) HttpStatusCode.ProxyAuthenticationRequired kivétel: hello ügyfél nem tudta hitelesíteni a hello proxykiszolgálót. 
   * ErrorCode.CiSApplianceDNSError – Ez jelzi a WebExceptionStatus.NameResolutionFailure kivétel: hello feloldó szolgáltatás nem tudta feloldani a hello állomásnevet.
   * ErrorCode.CiSApplianceACSError – Ez azt jelzi, hogy hello szolgáltatás hitelesítési hibát adott vissza, de nincs kapcsolat.
     
     Ha azt nem lépett egy webes kivétel, ellenőrizze, hogy ErrorCode.CiSApplianceFailure. Ez azt jelzi, hogy hello készülék nem sikerült.
5. Ellenőrizze a hello felhőalapú szolgáltatás kapcsolatot. Ha hello szolgáltatás webes kivételt okoz, a következő hibák hello merülhetnek fel:
   
   * ErrorCode.CiSApplianceGateway – Ez jelzi a HttpStatusCode.BadGateway kivétel: közbenső proxykiszolgálók hibás kérelmet kapott a másik proxy vagy hello eredeti kiszolgálóról.
   * ErrorCode.CiSApplianceProxy – Ez jelzi (HTTP-állapotkód 407) HttpStatusCode.ProxyAuthenticationRequired kivétel: hello ügyfél nem tudta hitelesíteni a hello proxykiszolgálót. 
   * ErrorCode.CiSApplianceDNSError – Ez jelzi a WebExceptionStatus.NameResolutionFailure kivétel: hello feloldó szolgáltatás nem tudta feloldani a hello állomásnevet.
   * ErrorCode.CiSApplianceACSError – Ez azt jelzi, hogy hello szolgáltatás hitelesítési hibát adott vissza, de nincs kapcsolat.
     
     Ha azt nem lépett egy webes kivétel, ellenőrizze, hogy ErrorCode.CiSApplianceSaasServiceError. Ez a StorSimple Manager szolgáltatás hello hibáját jelzi.
6. Ellenőrizze az Azure Service Bus-kapcsolatot. ErrorCode.CiSApplianceServiceBusError azt jelzi, hogy adott hello eszközről nem lehet kapcsolódni a Service Bus toohello.

naplófájlok CiSCommandletLog0Curr.errlog hello pedig a CiSAgentsvc0Curr.errlog további információkat, például a kivétel részletei. 

Hogyan toouse hello parancsmaggal kapcsolatos további információkért lépjen túl[Test-HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx) a Windows PowerShell hello dokumentáció.

> [!IMPORTANT]
> Ez a parancsmag hello aktív és passzív vezérlő hello is futtathatja. 
> 
> 

Lásd: a minta kimenet követően – hello hello `Test-HcsmConnection` parancsmag. 

**Minta kimenet – sikeresen regisztrált eszköz StorSimple Release fut (2014. július)**

hello első minta egy eszközről, a StorSimple Manager szolgáltatás hello regisztrálása sikeres volt, és kialakított kapcsolat van. 

     Controller1>Test-HcsmConnection -verbose
     Checking device state  ... Success.
     Device registered successfully
     Checking device authentication.  ... This operation will take few minutes toocomplete....
     Checking device authentication.  ... Success.
     Checking connectivity from device tooStorSimple Manager service.  ... This operation will take few minutes toocomplete....
     Checking connectivity from device tooStorSimple Manager service.  ... Success.
     Checking connectivity from StorSimple Manager service tooStorSimple device. .... Success.
     Controller1>

**Minta kimenet – StorSimple 1. frissítést futtató sikeresen regisztrált eszközre**

Ha a StorSimple eszköz frissítése 1 futtatja, nem kell toorun hello részletes kapcsolóval együtt.

      Controller1>Test-HcsmConnection

      Checking device registration state  ... Success
      Device registered successfully

      Checking primary NTP server [time.windows.com] ... Success

      Checking web proxy  ... NOT SET

      Checking primary IPv4 DNS server [10.222.118.154] ... Success
      Checking primary IPv6 DNS server  ... NOT SET
      Checking secondary IPv4 DNS server [10.222.120.24] ... Success
      Checking secondary IPv6 DNS server  ... NOT SET

      Checking device online  ... Success

      Checking device authentication  ... This will take a few minutes.
      Checking device authentication  ... Success

      Checking connectivity from device tooservice  ... This will take a few minutes.

      Checking connectivity from device tooservice  ... Success

      Checking connectivity from service toodevice  ... Success

      Checking connectivity tooMicrosoft Update servers  ... Success
      Controller1>

**Minta kimenet – StorSimple Release futtató offline eszköz (2014. július)**

Ez a minta egy eszközről, amely állapota van **Offline** a hello a klasszikus Azure portálon.

     Checking device state: Success 
     Device is registered successfully 
     Checking connectivity from device tooSaaS.. Failure

hello eszközt nem sikerült csatlakozni hello aktuális webproxy-konfigurációja. Ennek oka lehet a webproxy-konfigurációja hello problémát vagy a hálózati csatlakozási probléma. Ebben az esetben meg kell győződnie arról, hogy a webes proxykiszolgáló beállításainak helyességét és a webproxy-kiszolgálók online és elérhető-e. 

## <a name="troubleshoot-with-hello-sync-hcstime-cmdlet"></a>Hello Sync-HcsTime parancsmaggal hibaelhárításához
Ez a parancsmag toodisplay hello eszköz idő használata. Ha hello eszköz idő eltolással hello NTP-kiszolgálóval rendelkezik, használhatja a parancsmag tooforce-hello idő szinkronizálása a NTP-kiszolgáló. Hello eltolás hello eszköz és az NTP-kiszolgáló közötti érték nagyobb, mint 5 perc, ha megjelenik egy figyelmeztetés. Ha hello eltolás meghaladja a 15 perc, hello eszköz kapcsolat nélküli kerül. Ez a parancsmag tooforce egy időszinkronizálást továbbra is használhatja. Azonban ha hello eltolás meghaladja a 15 óra, akkor nem lesz képes tooforce-szinkronizálás hello ideje, és jelenik meg hibaüzenet.

**Minta kimenet – segítségével a Sync-HcsTime kényszerített az idő szinkronizálása**

     Controller0>Sync-HcsTime
     hello current device time is 4/24/2015 4:05:40 PM UTC.

     Time difference between NTP server and appliance is 00.0824069 seconds. Do you want tooresync time with NTP server?
     [Y] Yes [N] No (Default is "Y"): Y
     Controller0>

## <a name="troubleshoot-with-hello-enable-hcsping-and-disable-hcsping-cmdlets"></a>A hello Enable-HcsPing és a Disable-HcsPing parancsmaggal hibaelhárításához
Ezen parancsmagok tooensure, hogy az eszköz hálózati csatolóinak hello válaszol-e tooICMP ping kérelem használja. Alapértelmezés szerint hello StorSimple hálózati adapterek nem válaszolnak tooping kérelmeket. Hello legegyszerűbb módja tooknow ennek a parancsmagnak a használata, ha az eszköz online és elérhető-e.  

**Minta kimenet – Enable-HcsPing és a Disable-HcsPing**

     Controller0>
     Controller0>Enable-HcsPing
     Successfully enabled ping.
     Controller0>
     Controller0>
     Controller0>Disable-HcsPing
     Successfully disabled ping.
     Controller0>

## <a name="troubleshoot-with-hello-trace-hcsroute-cmdlet"></a>Hello nyomkövetési-HcsRoute parancsmaggal hibaelhárításához
Ez a parancsmag olyan útvonal eszköz használható. Hello módon tooa végső rendeltetési tooeach útválasztó csomagokat küld egy meghatározott időtartamra vonatkozóan, és majd kiszámítja a hello csomagok egyes ugrások által visszaadott eredmény. Hello parancsmag adott útválasztóról vagy hivatkozás csomagvesztés hello bizonyos fokú jeleníti meg, mert mely útválasztók tudja, vagy előfordulhat, hogy a hivatkozások hálózati problémát okoz.

**Minta kimenet látható, hogyan tootrace hello nyomkövetési-HcsRoute csomagot útvonala**

     Controller0>Trace-HcsRoute -Target 10.126.174.25

     Tracing route toocontoso.com [10.126.174.25]
     over a maximum of 30 hops:
       0  HCSNode0 [10.126.173.90]
       1  contoso.com [10.126.174.25]

     Computing statistics for 25 seconds...
                 Source tooHere   This Node/Link
     Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  Address
       0                                           HCSNode0 [10.126.173.90]
                                     0/ 100 =  0%   |
       1    0ms     0/ 100 =  0%     0/ 100 =  0%  contoso.com
      [10.126.174.25]

     Trace complete.

## <a name="troubleshoot-with-hello-get-hcsroutingtable-cmdlet"></a>Hello Get-HcsRoutingTable parancsmaggal hibaelhárításához
Ez a parancsmag tooview hello útvonaltábla használata a StorSimple eszközt. Útválasztási táblázat olyan szabályok, amelyek segíthetnek meghatározni, ha az Internet Protocol (IP) hálózaton keresztül továbbított adatok csomagokat a rendszer kéri. 

hello útválasztási táblázat hello felületek és hello átjáró, hogy útvonalak hello adatok toohello megadott hálózatok. Hello útválasztási metrika, amely hello döntéshozó a hello útvonalán tooreach egy adott célra is biztosít. hello alacsonyabb hello útválasztási metrika, hello magasabb hello prioritása. 

Például, ha 2 hálózati adapter áll rendelkezésükre, DATA 2 és a DATA 3, a csatlakoztatott toohello Internet rendelkezik. Ha DATA 2 és a DATA 3 útválasztási metrikáját hello 15 és 261 rendre, majd DATA 2 hello alacsonyabb útválasztási metrikájú a hello előnyben részesített felület használt tooreach hello Internet.

Ha a StorSimple eszköz frissítése 1 futtatja, a DATA 0 hálózati adapterén rendelkezik hello felhőforgalom hello legmagasabb beállításait. Ez azt jelenti, hogy akkor is, ha nincsenek más felhőalapú felülettel, hello felhőalapú forgalom lesz átirányítva a DATA 0. 

Ha futtatja a hello `Get-HcsRoutingTable` paramétereket (a példa azt mutatja meg a következő hello), a parancsmag hello megadása nélkül parancsmag kimeneteként IPv4 és IPv6-útválasztási táblázataiba. Másik lehetőségként megadhat `Get-HcsRoutingTable -IPv4` vagy `Get-HcsRoutingTable -IPv6` tooget a megfelelő útválasztási táblázatban.

      Controller0>
      Controller0>Get-HcsRoutingTable
      ===========================================================================
      Interface List
       14...00 50 cc 79 63 40 ......Intel(R) 82574L Gigabit Network Connection
       12...02 9a 0a 5b 98 1f ......Microsoft Failover Cluster Virtual Adapter
       13...28 18 78 bc 4b 85 ......HCS VNIC
        1...........................Software Loopback Interface 1
       21...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
       22...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #3
      ===========================================================================

      IPv4 Route Table
      ===========================================================================
      Active Routes:
      Network Destination        Netmask          Gateway       Interface  Metric
                0.0.0.0          0.0.0.0  192.168.111.100  192.168.111.101     15
              127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
              127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
        127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
            169.254.0.0      255.255.0.0         On-link     169.254.1.235    261
          169.254.1.235  255.255.255.255         On-link     169.254.1.235    261
        169.254.255.255  255.255.255.255         On-link     169.254.1.235    261
          192.168.111.0    255.255.255.0         On-link   192.168.111.101    266
        192.168.111.101  255.255.255.255         On-link   192.168.111.101    266
        192.168.111.255  255.255.255.255         On-link   192.168.111.101    266
              224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
              224.0.0.0        240.0.0.0         On-link     169.254.1.235    261
              224.0.0.0        240.0.0.0         On-link   192.168.111.101    266
        255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        255.255.255.255  255.255.255.255         On-link     169.254.1.235    261
        255.255.255.255  255.255.255.255         On-link   192.168.111.101    266
      ===========================================================================
      Persistent Routes:
        Network Address          Netmask  Gateway Address  Metric
                0.0.0.0          0.0.0.0  192.168.111.100       5
      ===========================================================================

      IPv6 Route Table
      ===========================================================================
      Active Routes:
       If Metric Network Destination      Gateway
        1    306 ::1/128                  On-link
       13    276 fd99:4c5b:5525:d80b::/64 On-link
       13    276 fd99:4c5b:5525:d80b::1/128
                                          On-link
       13    276 fd99:4c5b:5525:d80b::3/128
                                          On-link
       13    276 fe80::/64                On-link
       12    261 fe80::/64                On-link
       13    276 fe80::17a:4eba:7c80:727f/128
                                          On-link
       12    261 fe80::fc97:1a53:e81a:3454/128
                                          On-link
        1    306 ff00::/8                 On-link
       13    276 ff00::/8                 On-link
       12    261 ff00::/8                 On-link
       14    266 ff00::/8                 On-link
      ===========================================================================
      Persistent Routes:
        None

      Controller0>

## <a name="step-by-step-storsimple-troubleshooting-example"></a>StorSimple részletes hibaelhárítási példa
hello következő példa bemutatja a StorSimple központi telepítés részletes hibaelhárítási. Hello a példaforgatókönyvben eszközök regisztrációja sikertelen, és egy hiba üzent arról, hogy hello DNS-név vagy hello hálózati beállítások helytelen.

hello hibaüzenet jelenik a következő:

     Invoke-HcsSetupWizard: An error has occurred while registering hello device. This could be due tooincorrect IP address or DNS name. Please check your network settings and try again. If hello problems persist, contact Microsoft Support.
     +CategoryInfo: Not specified
     +FullyQualifiedErrorID: CiSClientCommunicationErros, Microsoft.HCS.Management.PowerShell.Cmdlets.InvokeHcsSetupWizardCommand

hello következő hello hiba lehetséges okok:

* Helytelen hardver telepítése
* Hibás hálózati adaptert
* Helytelen IP-cím, alhálózati maszk, átjáró, elsődleges DNS-kiszolgáló vagy webes proxy
* Nem megfelelő regisztrációs kulcs
* Helytelen tűzfal beállításai

### <a name="toolocate-and-fix-hello-device-registration-problem"></a>toolocate és javítás hello eszköz regisztrációs probléma
1. Ellenőrizze az eszköz konfigurációját: hello aktív vezérlő, futtassa `Invoke-HcsSetupWizard`.
   
   > [!NOTE]
   > hello beállítása varázsló hello aktív tartományvezérlőn kell futtatni. csatlakoztatott toohello aktív vezérlő, nézze meg hello soros konzolon megjelenített hello szalagcím képező tooverify. hello szalagcím azt jelzi, hogy csatlakoztatott toocontroller 0 vagy 1. vezérlő, és hogy hello vezérlő aktív vagy passzív-e. További információ: túl[azonosíthatja az aktív vezérlőhöz az eszközön](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).
   > 
   > 
2. Győződjön meg arról, hogy megfelelően van-e csatlakoztatva hello eszköz: hello eszközön vissza vezérlősík kábelek hello hálózati ellenőrzése. adott toohello eszközmodell hello kábelek. További információ: túl[a StorSimple 8100 eszköz telepítése](storsimple-8100-hardware-installation.md) vagy [a StorSimple 8600 eszköz telepítése](storsimple-8600-hardware-installation.md).
   
   > [!NOTE]
   > Ha 10 GbE hálózati portokat használ, szüksége lesz a megadott QSFP-SFP és SFP toouse hello. További információkért lásd: hello [kábelek, kapcsolók és adó-vevők ajánlott hello 10 GbE portok listája](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
   > 
   > 
3. Hello hálózati illesztő hello állapotának ellenőrzése:
   
   * Hello Get-NetAdapter parancsmag toodetect hello állapotfigyelő hello hálózati adapterek használata a DATA 0. 
   * Hello kapcsolat nem működik, ha hello **ifindex** állapotát jelzi, hogy hello illesztő nem működik. Szüksége lesz majd toocheck hello hálózati kapcsolat hello port toohello készülék és toohello kapcsoló. Kimenő hibás kábelek toorule is szüksége lesz. 
   * Amennyiben azt gyanítja, hogy hello sikertelen volt a port hello aktív vezérlő DATA 0, akkor ezt úgy ellenőrizheti csatlakozás toohello DATA 0 vezérlő 1 portot. tooconfirm, válassza le hello hálózati kábel hello hátsó hello eszköz vezérlő 0, csatlakozzon hello kábel toocontroller 1, és futtassa újból a Get-NetAdapter hello parancsmag. 
     Ha a DATA 0 port tartományvezérlőn sikertelen lesz, hello [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) további lépéseket. Tooreplace hello vezérlő akkor lehet szükség a rendszeren.
4. Ellenőrizze a hello kapcsolat toohello kapcsoló:
   
   * Győződjön meg arról, hogy a vezérlő 0 és 1. az elsődleges szolgáltatással vezérlő DATA 0 hálózati adapterek vannak a hello ugyanazon az alhálózaton. 
   * Ellenőrizze a hello központ vagy az útválasztó. Általában csatlakoznia mindkét tartományvezérlők toohello azonos hub vagy útválasztó. 
   * Győződjön meg arról, használhatja a hello kapcsolat hello kapcsolók DATA 0 mindkét hello tartományvezérlőinek az ugyanazon vLAN.
5. Bármely felhasználó hibák kiküszöbölése:
   
   * Futtassa újra a hello beállítása varázsló (futtatása **Invoke-HcsSetupWizard**), és írja be hello értékek újra toomake meg arról, hogy nincsenek-e hibák. 
   * Hello regisztráció-ellenőrzés használt kulcs. azonos regisztrációs kulcs hello lehet használt tooconnect több eszközök tooa StorSimple Manager szolgáltatás. A hello eljárással [hello Szolgáltatásregisztrációs kulcs lekérése](storsimple-manage-service.md#get-the-service-registration-key) tooensure, Ön által használt hello megfelelő regisztrációs kulcsot.
     
     > [!IMPORTANT]
     > Ha több szolgáltatás fut, akkor tooensure hello regisztrációs kulcs hello megfelelő service érték használt tooregister hello eszköz. Ha hello helytelen StorSimple Manager szolgáltatásban regisztrált egy eszközt, akkor túl[forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) további lépéseket. Előfordulhat, hogy tooperform (amely adatvesztést eredményezhet) hello eszköz gyári beállításainak visszaállítását toothen csatlakoztassa szánt toohello szolgáltatás.
     > 
     > 
6. Használja a kapcsolat tesztelése hello parancsmag tooverify, hogy rendelkezik-e kapcsolat toohello hálózaton kívülről. További információ: túl[hello Test-Connection parancsmaggal kapcsolatos problémák elhárítása](#troubleshoot-with-the-test-connection-cmdlet).
7. Ellenőrizze, hogy a tűzfal zavaró tényező. Ha ellenőrizte, hogy hello virtuális IP-cím (VIP), az alhálózati, az átjáró és a DNS-beállítások helyességét, és problémák továbbra is megjelenik, majd lehetséges, hogy a tűzfal blokkolja az eszközön, és a hálózaton kívül hello közötti kommunikáció. Meg kell, hogy elérhetők-e 80-as és 443-as port kimenő kommunikációra a StorSimple eszköz tooensure. További információkért lásd: [a StorSimple eszköz hálózatkezelési követelményei](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
8. Tekintse meg hello naplókat. Nyissa meg túl[támogatja a csomagok és a rendelkezésre álló eszköznaplók hibaelhárítási](#support-packages-and-device-logs-available-for-troubleshooting).
9. Ha hello előző lépések nem oldja meg hello probléma [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) segítségért.

## <a name="next-steps"></a>Következő lépések
[Megtudhatja, hogyan tootroubleshoot egy operatív eszköz](storsimple-troubleshoot-operational-device.md).

<!--Link references-->

[1]: https://technet.microsoft.com/library/dd379547(v=ws.10).aspx
[2]: https://technet.microsoft.com/library/dd392266(v=ws.10).aspx 
