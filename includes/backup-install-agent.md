## <a name="download-install-and-register-hello-azure-backup-agent"></a>Letöltése, telepítése és regisztrálása hello Azure Backup szolgáltatás ügynöke
Miután létrehozta a hello Azure Backup-tárolóban, az ügynök kell telepíteni mindegyik a Windows gép (Windows Server, a Windows ügyfél, a System Center Data Protection Manager-kiszolgáló vagy Azure biztonsági mentési kiszolgálóként működő számítógép), amely lehetővé teszi, hogy készítsen biztonsági másolatot az adat és alkalmazás tooAzure.

1. Jelentkezzen be toohello [felügyeleti portálon](https://manage.windowsazure.com/)
2. Kattintson a **Recovery Services**, majd válassza ki, hogy szeretné-e a kiszolgálóval tooregister hello mentési tároló. a mentési tároló hello gyors üzembe helyezés lap jelenik meg.
   
    ![Első lépések](./media/backup-install-agent/quickstart.png)
3. A hello gyors kezdés lapon kattintson a hello **For Windows Server, a System Center Data Protection Manager vagy a Windows ügyfél** lehetőség alatt **ügynök letöltése**. Kattintson a **mentése** toocopy azt toohello helyi számítógép.
   
    ![Ügynök mentése](./media/backup-install-agent/agent.png)
4. Hello ügynök telepítése után kattintson duplán a MARSAgentInstaller.exe toolaunch hello telepítési hello Azure Backup szolgáltatás ügynökének. Válassza ki a hello telepítési mappát és az ideiglenes mappa hello ügynök szükséges. hello gyorsítótár helyére szabad területet, amely a biztonsági mentési adatok hello legalább 5 %-át kell rendelkeznie.
5. Ha a proxy server tooconnect toohello hello az internet **proxykonfigurációt** képernyőn, írja be a hello proxy kiszolgáló adatait. Ha egy hitelesített proxykiszolgálót használ, meg hello felhasználói nevet és jelszót adatait ezen a képernyőn.
6. hello Azure Backup szolgáltatás ügynökének telepíti a .NET-keretrendszer 4.5 és Windows PowerShell (Ha még nincs elérhető) toocomplete hello telepítés.
7. Hello ügynök telepítése után kattintson a hello **tooRegistration folytatható** gomb toocontinue hello a munkafolyamathoz.
   
   ![Regisztráljon](./media/backup-install-agent/register.png)
8. Keresse meg tooand válassza hello tároló hitelesítési adatait tartalmazó fájlt, amely korábban letöltött hello tárolói hitelesítő adatok képernyőn.
   
    ![Tárolói hitelesítő adatokat](./media/backup-install-agent/vc.png)
   
    hello tárolói hitelesítő adatok fájlja csak esetén érvényes 48 óra (miután hello portálról letöltés). Ha valamilyen hiba ezen a képernyőn (például "megadott tárolói hitelesítő adatok fájl lejárt"), Azure-portál bejelentkezési toohello és letöltési hello tárolói hitelesítő adatokat fájl újra.
   
    Győződjön meg arról, hogy hello tároló hitelesítési adatait tartalmazó fájlt elérhető egy helyen hello telepítő alkalmazás is elérhetők. Ha eléréséhez kapcsolódó hibák, ez gépen hello tárolói hitelesítő adatok fájl tooa ideiglenes helyre másolja, majd próbálja megismételni hello műveletet.
   
    Ha érvénytelen tárolói hitelesítő adatok hibát észlel (például "érvénytelen megadott tárolói hitelesítő adatok") hello fájl sérült, vagy nem nem hello legújabb hitelesítő adatokat társított hello szolgáltatásban. Zónák hello egy új tárolói hitelesítő adatok fájlját letöltése hello portálról. Ez a hiba általában látható, ha hello kattintva hello **letöltési tárolóhitelesítő adatokat** hello Azure-portálon, gyors egymásutánban beállítást. Ebben az esetben csak hello második tárolói hitelesítő adatok fájlját esetén érvényes.
9. A hello **titkosítási beállítás** képernyő, megadhat egy hozzáférési kódot létrehozni, vagy adjon meg egy jelszót (minimum 16 karakter). Ne felejtse el toosave hello jelszót biztonságos helyen.
   
    ![Titkosítás](./media/backup-install-agent/encryption.png)
   
   > [!WARNING]
   > Ha hello jelszó elvesztése vagy elfelejtése; Nem a Microsoft hello biztonsági mentési adatok helyreállítása a Súgó. hello végfelhasználói hello titkosítási jelszó tulajdonában van, ezért a Microsoft nem látnak bele hello végfelhasználói által használt hello jelszót. Mentse hello fájlt biztonságos helyen szükség van egy helyreállítási művelet során.
   > 
   > 
10. Hello gombra kattintva **Befejezés** gombra, hello számítógép regisztrálása sikeres toohello tárolóban, és most már áll készen toostart tooMicrosoft Azure biztonsági mentéséről.
11. A Microsoft Azure Backup szolgáltatás önálló használata esetén hello során megadott beállítások hello regisztrációs munkafolyamat hello kattintva módosíthatja **tulajdonságainak módosítása** hello Azure Backup szolgáltatás mmc beépülő modul beállítást.
    
    ![Tulajdonságainak módosítása](./media/backup-install-agent/change.png)
    
    Azt is megteheti, a Data Protection Manager használata esetén is hello beállítások módosítása gombra kattintva hello hello regisztrációs munkamenet során megadott **konfigurálása** lehetőség kiválasztásával **Online** hello alatt **Felügyeleti** fülre.
    
    ![Az Azure Backup konfigurálása](./media/backup-install-agent/configure.png)

