|Paraméter neve| Típus | Leírás| Lehetséges értékek|
|-|-|-|-|
| /ServerMode|Kötelező|Adja meg e két hello konfigurációs és a folyamat kiszolgálón kell telepíteni, vagy csak a hello folyamatkiszolgáló|CS<br>PS|
|/InstallLocation|Kötelező|hello mappa mely hello összetevőinek telepítése| Egyetlen mappa hello számítógépen|
|/MySQLCredsFilePath|Kötelező|hello fájl elérési útját, mely hello MySQL a kiszolgáló hitelesítő adatok tárolása|hello fájlnak kell lennie az alább megadott hello formátumban|
|/VaultCredsFilePath|Kötelező|hello fájl elérési útját hello tárolói hitelesítő adatok|Érvényes fájlelérési út|
|/EnvType|Kötelező|Típusát, amelyet az tooprotect környezete |VMware<br>NonVMware|
|/PSIP|Kötelező|Hello NIC toobe replikációs adatátvitelhez használt IP-címe| Bármilyen érvényes IP-cím|
|/CSIP|Kötelező|mely hello a konfigurációs kiszolgáló figyel a következőn hello NIC hello IP-címe| Bármilyen érvényes IP-cím|
|/PassphraseFilePath|Kötelező|hello jelszót fájl teljes elérési útja toolocation hello|Érvényes fájlelérési út|
|/BypassProxy|Optional|Megadja, hogy hello konfigurációs kiszolgáló kapcsolódik tooAzure proxy nélkül|toodo Venu Ez az érték lekérése|
|/ProxySettingsFilePath|Optional|Proxybeállítások (hello alapértelmezett proxy igényel hitelesítést, vagy egy egyéni proxy)|hello fájl az alább megadott hello formátumban kell megadni|
|DataTransferSecurePort|Optional|A replikált adatok használt hello PSIP toobe lévő portszám| Érvényes portszám (az alapértelmezett érték 9433)|
|/SkipSpaceCheck|Optional|Gyorsítótárlemez terület-ellenőrzésének kihagyása| |
|/AcceptThirdpartyEULA|Kötelező|Ez a jelölő a külső féltől származó végfelhasználói licencszerződés elfogadását jelzi| |
|/ShowThirdpartyEULA|Optional|Külső felektől származó végfelhasználói licenszszerződés megjelenítése. Bemenetként való megadása esetén figyelmen kívül hagyja a többi paramétert.| |
