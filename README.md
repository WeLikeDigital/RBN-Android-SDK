RBN SDK для Android
==============

SDK позволяет подключить ваше приложение к Russian Beacon Network.

## Подключение SDK в Android-приложении

Для интеграции SDK в приложение, требуется импортировать библиотеку в проект. Для проектов, использующих систему сборки graddle:

```ruby
dependencies {
    compile 'ru.welike.beacons:library:0.9@aar' (пока не работает)
}
```

Либо вручную, добавив jar в зависимости проекта.

## Работа с SDK

### Инициализация SDK

1. Для инициализации и конфигурация SDK рекоммендуется переопределить класс Application и создать там объект типа WLBeaconsManager

```Java
	public class JuneApplication extends Application {
    private static final String BEACONS_API_KEY = "YOUR_API_KEY"; 
    private WLBeaconsManager beaconsManager;
    public WLBeaconsManager getBeaconsManager() {
        return beaconsManager;
    }
    @Override
    public void onCreate() {
        super.onCreate();
        beaconsManager = new WLBeaconsManager(this, BEACONS_API_KEY);
    }
}
```

Для конфигурации SDK можно создать объект класса WLBeaconsConfiguration и передать его в качестве параметра для WLBeaconsManager, либо воспользоваться методами класса WLBeaconsManager напрямую:

```Java
	WlBeaconsConfiguration config = new WLBeaconsConfiguration.Builder()
            .enableLogging(true)
            .enableBluetoothPromptDialog(true)
            .build();
  beaconsManager = new WLBeaconsManager(this, config);
  beaconsManager.setPrimaryColorResource(R.color.primary);
```

Для того, чтобы приложение могло получать и обрабатывать данные об акциях, необходимо установить слушатель событий(так же в методе onCreate класса Application):

```Java
beaconsManager.setOnEnterRegionListener(new OnEnterRegionListener() {
    @Override
    public void onEnterRegion(BeaconAdvert advert) {
    } });
```

После этого, как только пользователь войдет в зону действия акции, в вашем приложении сработает метод onEnterRegion(), в качестве параметра у которого будет объект класса BeaconAdvert, содержащий в себе всю информацию об акции. 


## Получение API ключа
Для получения уникального API ключа приложения свяжитесь с нами по адресу welike@welike.ru

## Требования

* Xcode 6 и iOS SDK 8
* iOS 7.0+ target deployment
