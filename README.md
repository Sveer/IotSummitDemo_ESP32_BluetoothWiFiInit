# Basic demo for M5stack for init device WiFi by GATT Bluetooth service
Простое демо для M5stack и ESP32 - как инициализировать параметры WiFi с помощью 
собственного GATT-сервиса на устройстве


Инициализация червиса
```

GATTS.registerService(
   "12345678-90ab-cdef-0123-456789abcdef",
   GATT.SEC_LEVEL_NONE,
   [ 
      //регистрируем свойство для сервиса
     ["22222222-90ab-cdef-0123-456789abcdef", GATT.PROP_RWNI(1, 1, 1, 0)],
   ],
   function svch(c, ev, arg) {

    if (ev === GATTS.EV_WRITE) {
         btStatus=arg.data; //ПОлучаем дланные по блютус
		 ssid = btStatus.slice(0, btStatus.indexOf(';'));
		 pass = btStatus.slice(btStatus.indexOf(';')+1);
		 
		 Cfg.set({wifi: {ap: {enable: false}, sta:{ssid:ssid, pass:pass, enable: true}}}); 
		 Cfg.set({mqtt: {server: '192.168.43.37',enable: true}}); 
       return GATT.STATUS_OK;
     } 
     rerurn GATT.STATUS_OK; //Тут надо отслеживать CONNECT/DISCONNECT. Для DEM отвечаем всегда ОК. Но лучше BAD_REQUEST
   });

```
"# IotSummitDemo_ESP32_BluetoothWiFiInit" 
"# IotSummitDemo_ESP32_BluetoothWiFiInit" 
