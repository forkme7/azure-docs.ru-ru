## <a name="view-device-telemetry-in-the-dashboard"></a>Просмотр данных телеметрии устройства на панели мониторинга
На панели мониторинга в решении для удаленного мониторинга можно просматривать данные телеметрии, отправляемые устройством в Центр Интернета вещей.

1. В браузере вернитесь на панель мониторинга решения для удаленного мониторинга, в левой панели щелкните **Устройства**, чтобы перейти к **списку устройств**.
2. В **списке устройств** отображается текущее состояние устройства — **Работает**. Если это не так, то на панели **Сведения об устройстве** щелкните **Включить устройство**.
   
    ![Просмотр состояния устройства][18]
3. Щелкните **Панель мониторинга**, чтобы вернуться на панель мониторинга, в раскрывающемся списке **Device to View** (Устройство для просмотра) выберите свое устройство, чтобы просмотреть его данные телеметрии. Телеметрия из примера приложения — 50 единиц данных о внутренней температуре, 55 единиц данных о наружной температуре и 50 единиц данных о влажности.
   
    ![Просмотр телеметрии устройства][img-telemetry]

## <a name="invoke-a-method-on-your-device"></a>Вызов метода на устройстве
На панели мониторинга в решении для удаленного мониторинга можно вызывать методы на своих устройствах через Центр Интернета вещей. Например, в решении для удаленного мониторинга можно вызвать метод, имитирующий перезагрузку устройства.

1. На панели мониторинга решения для удаленного мониторинга в левой панели щелкните **Устройства**, чтобы перейти к **списку устройств**.
2. Щелкните **Идентификатор устройства** для устройства в **списке устройств**.
3. На панели **Сведения об устройстве** щелкните **Методы**.
   
    ![Методы устройства][13]
4. В раскрывающемся списке **Метод** выберите **InitiateFirmwareUpdate**, а затем в поле **FWPACKAGEURI** введите фиктивный URL-адрес. Щелкните **Вызвать метод**, чтобы вызвать метод на устройстве.
   
    ![Вызов метода устройства][14]
   

5. Когда устройство обрабатывает метод, в консоли отображается сообщение о выполнении кода устройства. Результаты метода добавляются в журнал на портале решения:

    ![Просмотр журнала методов][img-method-history]

## <a name="next-steps"></a>Дополнительная информация
В статье [Настройка предварительно настроенного решения][lnk-customize] приведен ряд способов расширения этого примера. В их число входит использование реальных датчиков и реализация дополнительных команд.

Дополнительные сведения см. в статье [Разрешения на сайте azureiotsuite.com][lnk-permissions].

[13]: ./media/iot-suite-v1-visualize-connecting/suite4.png
[14]: ./media/iot-suite-v1-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-v1-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-v1-visualize-connecting/telemetry.png
[img-method-history]: ./media/iot-suite-v1-visualize-connecting/history.png
[lnk-customize]: ../articles/iot-suite/iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-v1-permissions.md
