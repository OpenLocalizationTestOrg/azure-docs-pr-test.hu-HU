
### <a name="installation-failures"></a>Telepítési hibák
| **Például szolgáló hibaüzenet** | **Javasolt művelet** |
|--------------------------|------------------------|
|Hiba történt sikertelen tooload fiókok. Hiba: System.IO.IOException: hello nem tooread adatait átviteli kapcsolat, ha telepítése és regisztrálása hello CS kiszolgáló.| Győződjön meg arról, hogy a TLS 1.0 hello számítógépen engedélyezve van-e. |

### <a name="registration-failures"></a>Regisztrációs hibák
Eszközregisztrációs hibák indítja el a hello hello naplók megtekintésével **%ProgramData%\ASRLogs** mappa.

| **Például szolgáló hibaüzenet** | **Javasolt művelet** |
|--------------------------|------------------------|
|**09:20:06**:InnerException.Type: SrsRestApiClientLib.AcsException,InnerException.<br>Üzenet: ACS50008: Az SAML-jogkivonat érvénytelen.<br>Nyomkövetési azonosító: 1921ea5b-4723-4be7-8087-a75d3f9e1072<br>Korrelációs azonosító: 62fea7e6-2197-4be4-a2c0-71ceb7aa2d97><br>Időbélyegző: **2016-12-12 14:50:08Z<br>** | Gondoskodjon arról, hogy a rendszer órája hello ideje nem hello helyi idő több mint 15 perc. Futtassa újra a hello telepítő toocomplete hello regisztrációs.|
|**09:35:27** : hello kiválasztott tanúsítványhoz tartozó összes vész helyreállítási tároló tooget közben DRRegistrationException:: Exception.Type:Microsoft.DisasterRecovery.Registration.DRRegistrationException kivételt okozott, Exception.Message: ACS50008: SAML-jogkivonat érvénytelen.<br>Nyomkövetési azonosító: e5ad1af1-2d39-4970-8eef-096e325c9950<br>Korrelációs azonosító: abe9deb8-3e64-464d-8375-36db9816427a<br>Időbélyegző: **2016-05-19 01:35:39Z**<br> | Gondoskodjon arról, hogy a rendszer órája hello ideje nem hello helyi idő több mint 15 perc. Futtassa újra a hello telepítő toocomplete hello regisztrációs.|
|06:28:45: sikertelen toocreate tanúsítvány<br>06:28:45:A telepítést nem lehet folytatni. A tanúsítvány nem hozható létre helyreállítási tooauthenticate tooSite szükséges. Telepítés ismételt futtatása | A telepítés futtatásához helyi rendszergazdaként jelentkezzen be. |
