UnifiedSetup.exe [/ ServerMode < CS/PS >] [/ InstallDrive <DriveLetter>] [/ MySQLCredsFilePath <MySQL credentials file path>] [/ VaultCredsFilePath <Vault credentials file path>] [/ EnvType < VMWare/NonVMWare >] [/ PSIP < IP-cím toobe használ az adatátvitelhez] [/CSIP <IP address of CS toobe registered with>] [/ PassphraseFilePath <Passphrase file path>]

Paraméterek:

* /ServerMode: Kötelező. Adja meg e két hello konfigurációs és a folyamat kiszolgálón kell telepíteni, vagy csak a hello folyamatkiszolgáló. Bemeneti értékek: CS, PS.
* Telepítési hely: Kötelező. hello mappa mely hello összetevők telepítése történik.
* /MySQLCredsFilePath. Kötelező. hello elérési mely hello MySQL server hitelesítő adatai vannak tárolva. hello fájlnak kell lennie a következő formátumban:
* [MySQLCredentials]
* MySQLRootPassword = "<Password>"
* MySQLUserPassword = "<Password>"
* /VaultCredsFilePath. Kötelező. hello tárolói hitelesítő adatok fájlját hello helye
* /EnvType. Kötelező. a telepítés hello típusát. Értékek: VMware, NemVMware
* /PSIP és /CSIP. Kötelező. hello IP-címe hello folyamatkiszolgáló és a konfigurációs kiszolgáló.
* /PassphraseFilePath. Kötelező. hello hello jelszót fájl helyét.
* /BypassProxy. Választható. Határozza meg, hogy hello konfigurációs kiszolgáló kapcsolódik tooAzure proxy nélkül.
* /ProxySettingsFilePath. Választható. A proxybeállítások (hello alapértelmezett proxy igényel hitelesítést, vagy egy egyéni proxy). hello fájlnak kell lennie a következő formátumban:
* [ProxyBeállítások]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address>"
* ProxyPort = "<Port>"
* ProxyUserName="<User Name>"
* ProxyPassword="<Password>"
* DataTransferSecurePort. Választható. replikációs adatok hello portszámát.
* SkipSpaceCheck. Választható. Gyorsítótár terület-ellenőrzésének kihagyása.
* AcceptThirdpartyEULA. Kötelező. Hello külső EULA fogad el.
* ShowThirdpartyEULA. Kötelező. Külső felektől származó végfelhasználói licenszszerződés megjelenítése. Bemenetként való megadása esetén figyelmen kívül hagyja a többi paramétert.
