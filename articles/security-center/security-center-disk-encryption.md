---
title: "az Azure virtuális gép aaaEncrypt |} Microsoft Docs"
description: "Ez a dokumentum segít az Azure virtuális gép tooencrypt riasztást követő titkosításában az Azure Security Center."
services: security, security-center
documentationcenter: na
author: TomShinder
manager: swadhwa
editor: 
ms.assetid: f6c28bc4-1f79-4352-89d0-03659b2fa2f5
ms.service: security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/15/2017
ms.author: tomsh
ms.openlocfilehash: 7c7c6eed39d16bde8a0dfaffe3a3331c58101634
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encrypt-an-azure-virtual-machine"></a>Azure virtuális gép titkosítása
Az Azure Security Center riasztást küld Önnek, ha azt észleli, hogy egyes virtuális gépek nincsenek titkosítva. Ezek a riasztások jelenik meg: magas súlyosságú és hello ajánlás tooencrypt van a virtuális gépek.

![Lemeztitkosításra vonatkozó javaslat](./media/security-center-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> a dokumentumban szereplő információk hello tooencrypting virtuális gépek vonatkozik egy fő titkosítási kulcs (ez szükséges az Azure Backup használatával virtuális gépek biztonsági mentését) használata nélkül. Hello cikke [lemez titkosítás a Windows Azure és a Linux Azure Virtual Machines](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) kapcsolatos információk toouse egy fő titkosítási kulcs toosupport Azure Backup titkosított Azure virtuális gépeken.
>
>

tooencrypt, titkosítást igénylő Azure Security Center által azonosított Azure virtuális gépek, ajánlott hello a következő lépéseket:

* Az Azure PowerShell telepítése és konfigurálása. Ez lehetővé teszi az toorun hello PowerShell parancsok szükséges tooset hello Előfeltételek szükséges tooencrypt Azure virtuális gépek fel.
* Szerezzen be és hello Azure lemez titkosítási Előfeltételek Azure PowerShell-parancsprogrammal
* A virtuális gépek titkosítása

hello a jelen dokumentum célja tooenable meg tooencrypt a virtuális gépek, még akkor is, ha rendelkezik kevéssé vagy egyáltalán ne tapasztalattal az Azure PowerShell.
Ez a dokumentum feltételezi, hogy a Windows 10, amelyből Azure Disk Encryption konfigurálja hello ügyfél gépként.

Nincsenek használt toosetup hello Előfeltételek és az Azure virtuális gépek tooconfigure titkosítási számos különböző módszer. Ha már jól ismeri az Azure PowerShell vagy Azure CLI-t, majd célszerű toouse más megközelítést választani.

> [!NOTE]
> További információ az Azure virtuális gépek más megközelítést választani tooconfiguring titkosítási toolearn talál [lemez titkosítás a Windows Azure és a Linux Azure Virtual Machines](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).
>
>

## <a name="install-and-configure-azure-powershell"></a>Az Azure PowerShell telepítése és konfigurálása
Először telepítenie kell a számítógépre az Azure PowerShell 1.2.1-es vagy újabb verzióját. hello cikk [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) tooprovision a számítógép toowork az Azure PowerShell van szüksége a hello lépéseket tartalmazza. hello legegyszerűbb megoldás, toouse hello Web PI telepítési módszert, ha a cikkben említett. Ha már rendelkezik Azure PowerShell telepítése, újra használatával hello Web PI módszerrel, így hello Azure PowerShell legújabb verzióját kell telepíteni.

## <a name="obtain-and-run-hello-azure-disk-encryption-prerequisites-configuration-script"></a>Szerezzen be és hello Azure disk Encryption titkosítási előfeltétel-konfigurációs parancsfájl futtatása
hello Azure lemez titkosítási előfeltétel-konfigurációs parancsprogram az Azure virtuális gépek titkosításához szükséges összes hello Előfeltételek beállításához.

1. Nyissa meg toohello GitHub-oldalon, amelynek hello [Azure lemez titkosítási előfeltétel-beállító parancsprogramot](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).
2. A hello Github-lapon kattintson a hello **Raw** gombra.
3. Használjon **CTRL-A** tooselect összes hello hello oldalon szöveget, majd **CTRL-C** toocopy összes hello hello lap toohello vágólapon szöveg.
4. Nyissa meg **Jegyzettömb** és illessze be a Jegyzettömbbe hello másolt szöveget.
5. Hozzon létre egy új mappát a C: meghajtón **AzureADEScript** néven.
6. Mentés hello Jegyzettömb-fájlt – kattintson **fájl**, majd kattintson a **Mentés másként**. Hello Fájlnév szövegmezőben adja meg **"ADEPrereqScript.ps1"** kattintson **mentése**. (Ellenőrizze, hogy idézőjelbe hello hello neve, máskülönben fogja menteni hello fájl .txt a kiterjesztés).

Most, hogy hello parancsfájl tartalmak mentésekor, nyissa meg a hello parancsfájlt a PowerShell ISE hello:

1. Hello Start menüben, kattintson **Cortana**. Kérje meg **Cortana** írja be a "PowerShell" **PowerShell** a hello Cortana keresőmezőjébe.
2. Kattintson jobb gombbal a **Windows PowerShell ISE** elemre, majd válassza a **Futtatás rendszergazdaként** lehetőséget.
3. A hello **rendszergazda: Windows PowerShell ISE** ablak, kattintson a **nézet** , majd **parancsfájl ablaktábla megjelenítése**.
4. Ha megjelenik a hello **parancsok** hello ablak jobb oldalán hello ablaktábláján kattintson a hello **"x"** a hello jobb felső sarkában hello ablaktábla tooclose azt. Ha hello szöveg túl kicsi az Ön toosee, akkor **CTRL + Hozzáadás** ("Hozzáadása" hello "+" szimbólumra az). Ha hello szöveg túl nagy, akkor **CTRL + mínusz** (kivonás hello "-" jel).
5. Kattintson a **File** (Fájl) menüre, majd az **Open** (Megnyitás) elemre. Keresse meg a toohello **C:\AzureADEScript** mappa és hello kattintson duplán arra a hello **ADEPrereqScript**.
6. Hello **ADEPrereqScript** tartalma ekkor meg kell jelennie a PowerShell ISE hello és könnyebb a különböző összetevők, például parancsok, paraméterek és változók lásd itt a színkódolás toohelp.

Ekkor megjelenik az alábbi ábra hello hasonlót.

![A PowerShell ISE ablaka](./media/security-center-disk-encryption/security-center-disk-encryption-fig2.png)

hello felső ablaktábla hivatkozott tooas hello "panelbe", és hello alsó ablaktáblában hivatkozott tooas hello "konzol". A cikk későbbi részeiben mi is használni fogjuk ezeket az elnevezéseket.

## <a name="run-hello-azure-disk-encryption-prerequisites-powershell-command"></a>Hello Azure disk encryption Előfeltételek PowerShell-parancs futtatása
hello Azure Disk Encryption titkosítási előfeltétel parancsfájl kérni fogja hello hello parancsfájl elindítása után a következő információkat:

* **Erőforráscsoport neve** - neve az erőforráscsoportot, amelyet az tooput hello hello kulcstárolót.  Hello nevet az új erőforráscsoport létrejön, ha még nem egyet a beírt néven. Ha már rendelkezik egy erőforráscsoport, amelyet az ebben az előfizetésben toouse, majd adja meg ennek az erőforráscsoportnak hello nevét.
* **Kulcs tároló neve** -a melyik titkosítási kulcsai elhelyezett toobe hello kulcstároló neve. Ha a megadott néven még nem található kulcstároló, a rendszer létrehoz egyet a beírt néven. Ha már rendelkezik a kulcstároló, amelyet az toouse, adja meg a létező a Key Vault hello hello nevét.
* **Hely** -hello Key Vault helye. Gondoskodjon arról, hogy titkosított hello Kulcstárolónak, valamint a virtuális gépek toobe hello ugyanazon a helyen. Ha nem tudja hello helyét, vannak-e lépések a cikk későbbi részében bemutatjuk, hogyan toofind ki.
* **Az Azure Active Directory-alkalmazásnév alapján** -hello Azure Active Directory-alkalmazás, amely lesz használt toowrite titkok toohello kulcstároló neve. Ha a megadott néven még nem létezik alkalmazás, a rendszer létrehoz egyet a beírt néven. Ha már rendelkezik egy Azure Active Directory-alkalmazás, amelyet az toouse, adja meg, hogy az Azure Active Directory-alkalmazás hello nevét.

> [!NOTE]
> Fejezetét toowhy, ha szüksége van-e toocreate egy Azure Active Directory-alkalmazás, lásd: *alkalmazás regisztrálása az Azure Active Directory* hello cikkben szakasz [Ismerkedés az Azure Key Vault](../key-vault/key-vault-get-started.md).
>
>

Hajtsa végre a következő lépéseket tooencrypt egy Azure virtuális gép hello:

1. Ha korábban bezárta a PowerShell ISE hello, nyissa meg a PowerShell ISE hello emelt szintű példánya. Útmutatás alapján hello cikkben korábban Ha nyissa meg a PowerShell ISE még nincs hello. Ha korábban bezárta a hello parancsfájlt, majd nyissa meg a hello **ADEPrereqScript.ps1** kattintva **fájl**, majd **nyissa meg a** és hello parancsfájl kijelölésével hello **c:\ AzureADEScript** mappa. Ha ez a cikk hello indítás utasítást elvégezte, egyszerűen folytassa toohello következő lépésre.
2. A PowerShell ISE (hello PowerShell ISE hello alsó ablaktábláján) hello hello konzoljában módosítása hello fókusz toohello helyi hello parancsfájl beírásával **cd c:\AzureADEScript** nyomja le az ENTER **ENTER**.
3. Hello végrehajtási házirendjének beállítása a számítógépre, hogy hello parancsfájl futtatása. Típus **Set-ExecutionPolicy Unrestricted** a hello konzolon, és nyomja le az ENTER BILLENTYŰT. Ha egy hello tooexecution házirend hello hatásait kapcsolatos figyelmeztető párbeszédpanel jelenik meg, jelölje be az **tooall Igen** vagy **Igen** (Ha megjelenik **tooall Igen**, ez a lehetőség – akkor válassza, ha nem látható **tooall Igen**, majd kattintson a **Igen**).
4. Jelentkezzen be Azure-fiókjába. Írja be a konzolba hello **Login-AzureRmAccount** nyomja le az ENTER **ENTER**. Megjelenik egy párbeszédpanel, ahol megadta a hitelesítő adatait (Ellenőrizze, hogy a jogok toochange hello virtuális gépek – Ha a felhasználó nem rendelkezik jogosultsággal, csak akkor tudja tooencrypt őket. Ha nem biztos a dolgában, forduljon az előfizetés tulajdonosához vagy a rendszergazdához). Megjelennek a következő információk: **Environment**, **Account**, **TenantId**, **SubscriptionId** és **CurrentStorageAccount**. Másolás hello **SubscriptionId** tooNotepad. Szüksége lesz toouse #6. lépésben.
5. Milyen előfizetésre található a virtuális gép tartozik tooand helyére. Nyissa meg túl[https://portal.azure.com](ttps://portal.azure.com) , és jelentkezzen be.  Kattintson a bal oldalán található hello hello, **virtuális gépek**. Megjelenik egy lista a virtuális gépek és a hello előfizetéshez tartoznak.

   ![Virtuális gépek](./media/security-center-disk-encryption/security-center-disk-encryption-fig3.png)
6. Térjen vissza a PowerShell ISE toohello. Állítsa be a hello előfizetési kontextust, amelyben hello parancsprogram futni fog. Írja be a konzolba hello **Select-AzureRmSubscription – SubscriptionId < ön_előfizetés-azonosítója >** (cserélje le **< ön_előfizetés-azonosítója >** a tényleges előfizetés-Azonosítóval rendelkező), és nyomja le az ENTER  **Adja meg**. Látni fogja a hello környezetben információ **fiók**, **TenantId**, **SubscriptionId** és **CurrentStorageAccount**.
7. Most már áll készen toorun hello parancsfájl. Kattintson a hello **-parancsfájl futtatása** gombra vagy nyomja le az **F5** hello billentyűzet.

   ![A PowerShell-parancsprogram futtatása](./media/security-center-disk-encryption/security-center-disk-encryption-fig4.png)
8. hello parancsprogram kéri a **resourceGroupName:** -hello nevét adja meg *erőforráscsoport* kívánt toouse, majd nyomja le az **ENTER**. Ha még nincs fiókja, írjon be egy nevet az toouse egy újat. Ha már rendelkezik egy *erőforráscsoport* , hogy szeretné, hogy toouse (például, hogy a virtuális gép hello egyet be), adja meg a meglévő erőforráscsoportot hello hello nevét.
9. hello parancsprogram kéri a **keyVaultName:** -adjon meg hello hello *Key Vault* toouse szeretné, majd nyomja le az ENTER BILLENTYŰT. Ha még nincs fiókja, írjon be egy nevet az toouse egy újat. Ha már rendelkezik a kulcstároló, amelyet az toouse, beírása hello hello meglévő *Key Vault*.
10. hello parancsprogram kéri a **helye:** – hello helyet, mely hello a virtuális gép kívánt tooencrypt található hello nevét adja meg, majd nyomja le az ENTER **ENTER**. Ha nem emlékszik hello helyét, lépjen vissza toostep #5.
11. hello parancsprogram kéri a **aadAppName:** -adjon meg hello hello *Azure Active Directory* alkalmazás toouse, majd nyomja le az **ENTER**. Ha még nincs fiókja, írjon be egy nevet az toouse egy újat. Ha már rendelkezik egy *Azure Active Directory-alkalmazás* , hogy szeretné, hogy toouse, hello beírása hello meglévő *Azure Active Directory-alkalmazás*.
12. Megjelenik egy párbeszédpanel. Adja meg a hitelesítő adatait (Igen, egyszer már bejelentkezett, de a továbbiakban létre kell toodo újra).
13. hello parancsfájl fut, és amikor végzett, ekkor megkérdezi, hogy toocopy hello értékeinek hello **aadClientID**, **aadClientSecret**, **diskEncryptionKeyVaultUrl**, és **keyVaultResourceId**. Ezen értékek toohello vágólapra másolja és illessze be a Jegyzettömbbe.
14. Térjen vissza a PowerShell ISE toohello, és tegye oda a kurzort, hello hello végén lévő hello utolsó sort, és nyomja le az **ENTER**.

hello parancsfájl kimenete hello hasonlóan kell kinéznie üdvözlő képernyőt az alábbi:

![A PowerShell eredménye](./media/security-center-disk-encryption/security-center-disk-encryption-fig5.png)

## <a name="encrypt-hello-azure-virtual-machine"></a>Hello Azure virtuális gép titkosítása
Ön most már készen áll a tooencrypt vannak a virtuális gép. Ha a virtuális gép található hello ugyanabban az erőforráscsoportban mint a kulcstároló, továbbléphet a titkosításhoz szükséges lépésekre toohello. Azonban ha a virtuális gép nincs hello azonos erőforrás csoport mint a kulcstároló, szüksége lesz a tooenter hello következő, a PowerShell ISE hello hello konzolján:

**$resourceGroupName = &lt;Virtuális_gép_erőforráscsoportja&gt;**

Cserélje le **< virtuális_gép_erőforráscsoportja >** nevű hello hello az erőforráscsoportban, amelyben a virtuális gépek találhatók, egyetlen ajánlat körülvett. Nyomja le az **ENTER** billentyűt.
hello megfelelő erőforráscsoport-név lett megadva, a következőt a PowerShell ISE konzoljába hello hello tooconfirm:

**$resourceGroupName**

Nyomja le az **ENTER** billentyűt. A virtuális gépek található erőforráscsoport nevét hello kell megjelennie. Példa:

![A PowerShell eredménye](./media/security-center-disk-encryption/security-center-disk-encryption-fig6.png)

### <a name="encryption-steps"></a>A titkosítás lépései
Először tootell PowerShell hello neve hello virtuális gép kívánt tooencrypt. Hello konzol írja be:

**$vmName = &lt;’virtuális_gép_neve’&gt;**

Cserélje le **<' virtuális_gép_neve' >** hello nevű a virtuális gép (Győződjön meg a szimpla idézőjelbe között hello neve), és nyomja le az **ENTER**.

az tooconfirm hello megfelelő Virtuálisgép-név lett megadva, írja be:

**$vmName**

Nyomja le az **ENTER** billentyűt. Hello nevét kell megjelennie hello virtuális gép kívánt tooencrypt. Példa:

![A PowerShell eredménye](./media/security-center-disk-encryption/security-center-disk-encryption-fig7.png)

Nincsenek két módszer toorun hello titkosítási parancs tooencrypt összes meghajtó hello virtuális gépen. hello első módszer a következő parancsot a PowerShell ISE konzoljába hello tootype hello:

~~~
Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId -VolumeType All
~~~

A parancs beírását követően nyomja le az **ENTER** billentyűt.

hello második metódus tooclick hello parancsfájl ablaktáblán (hello hello PowerShell ISE felső paneljébe), és görgessen lefelé toohello hello script alján. Jelölje ki a hello fenti parancsot, és majd kattintson a jobb gombbal, és kattintson **kijelölés futtatása** vagy nyomja le az ENTER **F8** hello billentyűzet.

![PowerShell ISE](./media/security-center-disk-encryption/security-center-disk-encryption-fig8.png)

Függetlenül hello módszert használja, megjelenik egy párbeszédpanel, tájékoztatja arról, hogy hello művelet toocomplete 10 – 15 percig tart. Kattintson a **Yes** (Igen) gombra.

Hello titkosítási folyamat lefolyása közben vissza toohello Azure portálon, és hello virtuális gép hello állapotát tekintheti meg. Kattintson a bal oldalán található hello hello, **virtuális gépek**, ezt a hello **virtuális gépek** paneljén kattintson hello hello virtuális gép nevét, amelyet épp titkosít. Megjelenő hello panelen láthatja, hogy hello **állapot** mező értéke **Frissítéskísérleti**. Ez azt mutatja, hogy a titkosítás folyamatban van.

![Virtuális gép hello kapcsolatos további részletekért](./media/security-center-disk-encryption/security-center-disk-encryption-fig9.png)

Térjen vissza a PowerShell ISE toohello. Hello parancsprogram befejezését követően láthatja, mi hello az alábbi ábra jelenik meg.

![A PowerShell eredménye](./media/security-center-disk-encryption/security-center-disk-encryption-fig10.png)

Most, hogy a virtuális gép hello toodemonstrate titkosított térjen vissza a toohello Azure portálon, és kattintson **virtuális gépek** a bal oldalán található hello hello. Kattintson a korábban titkosított virtuális gép hello hello nevére. A hello **beállítások** panelen kattintson a **lemezek**.

![Beállítások](./media/security-center-disk-encryption/security-center-disk-encryption-fig11.png)

A hello **lemezek** panelen láthatja, hogy **titkosítási** van **engedélyezve**.

![Lemeztulajdonságok](./media/security-center-disk-encryption/security-center-disk-encryption-fig12.png)

## <a name="next-steps"></a>Következő lépések
Ebben a dokumentumban, megtudta, hogyan tooencrypt egy Azure virtuális gépen. További információ az Azure Security Center toolearn hello következő lásd:

* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello állapotát az Azure-erőforrások
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) -megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztások
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) – Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.
