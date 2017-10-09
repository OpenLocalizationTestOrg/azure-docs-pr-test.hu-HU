## <a name="associate-an-azure-storage-account-tooiot-hub"></a>Egy Azure Storage-fiók tooIoT Hub társítása

Mivel hello szimulált eszköz alkalmazásának feltölt egy fájlt tooa blobot, rendelkeznie kell egy [Azure Storage](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account) fiókhoz társított tooIoT központ. Egy Azure Storage-fiók társítása az IoT-központ, hello IoT-központ hoz létre egy SAS URI-t. Egy eszköz a fájl tooa blobtárolóban használhatja a SAS URI toosecurely feltöltése. az IoT-központ szolgáltatás hello és hello eszközoldali SDK-k koordinálja hello folyamat, amely hello SAS URI-t állít elő, és lehetővé teszi az elérhető tooa eszköz toouse tooupload egy fájl.

Hello utasításait követve [fájlfeltöltések konfigurálása hello Azure-portál használatával](../articles/iot-hub/iot-hub-configure-file-upload.md) tooassociate egy Azure Storage-fiók tooyour IoT-központot. Győződjön meg arról, hogy az IoT hub társítva-e a blob-tároló, és hogy engedélyezve vannak-e a fájl értesítések.

![A portál fájl értesítések engedélyezése](media/iot-hub-associate-storage/enable-file-notifications.png)