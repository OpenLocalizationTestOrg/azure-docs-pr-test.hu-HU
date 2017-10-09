## <a name="using-vault-credentials-tooauthenticate-with-hello-azure-backup-service"></a>Tárolói hitelesítő adatok tooauthenticate hello Azure Backup szolgáltatás használatával
hello a helyszíni kiszolgálónak (a Windows ügyfél vagy a Windows Server vagy a Data Protection Manager-kiszolgáló) kell toobe biztonsági mentését adatok tooAzure is azelőtt hitelesíthetők, a mentési tárolóban. hello hitelesítési érhető el "a hitelesítő adatokat tároló" használatával. hello tárolói hitelesítő adatokat fogalma egy Azure PowerShell használt "közzétételi beállítások" fájl hasonló toohello fogalmát.

### <a name="what-is-hello-vault-credential-file"></a>Mi az hello tárolóalapú hitelesítőadat-fájlt?
hello tárolói hitelesítő adatok fájlját az hello portal minden egyes biztonsági mentési tároló által létrehozott tanúsítványt. hello portal majd feltölti a hello nyilvános kulcs toohello Access Control Service (ACS). hello hello tanúsítvány titkos kulcsának elérhető toohello felhasználói hello munkafolyamat, amely bemenetként hello gép regisztrációs munkafolyamat részeként történik. Ez akkor hitelesíti a hello azonosított tooan gép toosend biztonsági mentési adatokat tároló hello Azure Backup szolgáltatás.

hello tároló hitelesítő adatait csak hello regisztrációs munkamenet során használt. Hello felhasználó felelőssége tooensure, amely hello tárolói hitelesítő adatokat a fájl ne legyenek veszélyben. Ha bármely rosszindulatú felhasználó hello tagoknál esik, hello tároló hitelesítési adatait tartalmazó fájlt lehet használt tooregister más gépek ellen hello azonos tárolóban. Azonban mivel hello biztonsági mentési adatok titkosítása a egy jelszót, amit toohello ügyfél tartozik, meglévő biztonsági mentési adatok nem sérült. toomitigate ezen probléma tárolói hitelesítő adatok vannak beállítva tooexpire 48hrs. Letöltheti a hello tárolói hitelesítő adatokat egy biztonsági mentési tároló tetszőleges számú alkalommal – azonban csak hello legújabb tárolói hitelesítő adatok fájlját alkalmazható hello regisztrációs munkamenet során.

### <a name="download-hello-vault-credential-file"></a>Hello tárolói hitelesítő adatok fájlját letöltése
hello tárolói hitelesítő adatok fájlját az Azure-portálon hello egy biztonságos csatornán keresztül letöltődik. hello Azure Backup szolgáltatás nem észleli a hello hello tanúsítvány titkos kulcsának és hello titkos kulcs nem őrzi meg hello portal vagy hello szolgáltatást. A következő lépéseket toodownload hello tárolói hitelesítő adatok fájl tooa helyi számítógép hello használata.

1. Jelentkezzen be toohello [felügyeleti portálon](https://manage.windowsazure.com/)
2. Kattintson a **Recovery Services** hello bal oldali navigációs panelen, és válassza hello mentési tároló, amely létrehozta. Kattintson a hello felhő ikon tooget toohello hello mentési tároló gyors kezdés ábrázolása.
   
   ![Gyors megtekintése](./media/backup-download-credentials/quickview.png)
3. Hello gyors kezdés lapon kattintson **letöltési tárolói hitelesítő adatokat**. hello portal hello tárolói hitelesítő adatok fájlját, amely letölthető állít elő.
   
   ![Letöltés](./media/backup-download-credentials/downloadvc.png)
4. hello portal kombinációjából hello tároló nevére és hello aktuális dátum tárolói hitelesítő adatot hoz létre. Kattintson a **mentése** toodownload hello tárolói hitelesítő adatok toohello helyi fiók Letöltések mappába, vagy a Mentés másként válassza ki a hello mentse menü toospecify hello tárolói hitelesítő adatok helyét.

### <a name="note"></a>Megjegyzés
* Győződjön meg arról, hogy hello tárolói hitelesítő adatokat is elérhetők, a gép helyre menti. Ha a megosztás/SMB fájl tárolja, ellenőrizze, hogy hello hozzáférési engedélyeket.
* hello tárolói hitelesítő adatok fájlja csak hello regisztrációs munkamenet során használt.
* hello tárolói hitelesítő adatok fájlját 48hrs után lejár, és hello portálról tölthető le.
* Tekintse meg az Azure Backup toohello [gyakran ismételt kérdések](../articles/backup/backup-azure-backup-faq.md) a hello munkafolyamat kapcsolatos kérdéseket.

