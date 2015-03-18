RBN SDK для Android
==============

SDK позволяет подключить ваше приложение к Russian Beacon Network.

## Подключение SDK в Android-приложении

Для интеграции SDK в приложение, требуется импортировать библиотеку в проект. Для проектов, использующих систему сборки graddle:

```ruby
repositories {
    mavenCentral()
    maven { url "https://raw.github.com/WeLikeDigital/RBN-Android-SDK/master" }
}
dependencies {
    compile 'org.altbeacon:android-beacon-library:2.1.2@aar'
    compile 'ru.welike.rbnsdk:library:0.9'
}
```

Либо вручную, добавив jar в зависимости проекта.

## Работа с SDK

### Инициализация SDK

1) Для работы RBNSDK в вашем приложении, вам потребуется API KEY, его следует указать в манифесте вашего приложения:
```XML
<application ... >
	<meta-data android:name="ru.welike.rbnsdk.appSecret" android:value="<YUOR_API_KEY>" />
</application>
```
2) Там же следует указать все разрешения, которые требуются для работы SDK:
```XML
	<!-- Выясняем имя пользователя для логов -->
	<uses-permission android:name="android.permission.GET_ACCOUNTS" />
	
	<!-- Координаты пользователя -->
	<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
	<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
	
	<!-- Интернет для отправки логов -->
	<uses-permission android:name="android.permission.INTERNET"/>
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
	
	<!-- Bluetooth для взаимодействия с биконами -->
	<uses-permission android:name="android.permission.BLUETOOTH"/>
	<uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
```

3) Для инициализации и конфигурация SDK рекоммендуется переопределить класс Application и проинициализировать там класс RBNManager
```Java
public class JuneApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        RBNManager.newInstance(this);
    }
}
```

Впоследствии можно всегда получить достум к менеджеру через 
```Java
RBNManager.getInstance();
```

Для конфигурации SDK можно создать объект класса WLBeaconsConfiguration и передать его в качестве параметра для WLBeaconsManager, либо воспользоваться методами класса WLBeaconsManager напрямую:
```Java
	WLBeaconsConfiguration config = new WLBeaconsConfiguration.Builder()
            .enableLogging(true)
            .enableBluetoothPromptDialog(true)
            .build();
  beaconsManager = new WLBeaconsManager(this, config);
  beaconsManager.setPrimaryColorResource(R.color.primary);
```

Для того, чтобы приложение могло получать и обрабатывать данные об акциях, необходимо установить слушатель событий(так же в методе onCreate класса Application):
```Java
RBNManager.getInstance().setOnEnterRegionListener(new OnEnterRegionListener() {
            @Override
            public void onEnterRegion(BeaconAdvert advert) {
            }
        });
```

После этого, как только пользователь войдет в зону действия акции, в вашем приложении сработает метод onEnterRegion(), в качестве параметра у которого будет объект класса BeaconAdvert, содержащий в себе всю информацию об акции. 
```Java
String title; // Заголовок объявления
String push_title; // Заголовок push-уведомления
String desc; // Описание объявления
String img; // Ссылка на обложку объявления
String btn_title; // Текст для кнопки действия
long start_time; // Время начала действия акции
long end_time; // Время конца действия акции
String duration; // Продолжительность акции (так и не понял какого черта это строка)
String url; // альтернатива "title, desc, img, btn_title", при октрытии пуша отркываем урл в браузере
String action_type;
String action_data;
String passbook; // ссылка на купон для пассбука
String shop_id;
String shop_floor;
boolean status;
```
## Получение API ключа
Для получения уникального API ключа приложения свяжитесь с нами по адресу welike@welike.ru

## Требования

* Android 4.0 и выше
