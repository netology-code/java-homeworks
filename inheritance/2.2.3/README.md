Домашнее задание по лекции 2.2. Основы ООП- модификаторы доступа, наследование
==

## Задача 3. Иерархия “Автомобили”
### Описание
На этот раз рассмотрим предметную область "Автомобили".
Рассмотрим проектирование приложения по размщению объявлений о продаже автомобилей. 
Реализуйте иерархию классов автомобилей с помощью наследования. В ней должны быть представлены три группы классов - 
по назначению (грузовые/легковые/пассажирские), по типу кузова (седан/универсал/грузовик/автобус),
по типу топлива (бензиновые, дизельные, гибридные и электрические).

Необходимо с помощью наследования реализовать программу, в которой будет один интерфейс `VehicleType`, три абстрактных реализаций интерфейса 
(`VehicleTypeByPurpose`, `VehicleTypeByBodyTypes`, `VehicleTypeByFuelTypes`), в которых будут определены аттрибуты каждой группы типов 
и на каждый абстрактный класс по несколько классов, в которых будет определены конкретное определение типа.

Данный функционал пригодится в случае массового фильтрации объявлений по какому-то искомому типу.

И так же необходимо будет описать класс `VehicleAd` с базовым набором полей, состоящим из id объявления, 
model (модели авто) и трех полей с каждым типом, 
и `AdsService`, в котором будет осуществляться фильтрация объявлений.

### Процесс реализации
1. Создайте Enum класс `VehicleTypeEnum` с 8 возможными жанрами в нашей программе
```
public enum VehicleTypeEnum {
    TRUCK,CAR,PASSENGER,SEDAN,WAGON,PICKUP,BUS,PETROL,DIESEL,HYBRID,ELECTRIC
}
```
2. Создайте интерфейс `VehicleType` с сигнатурами двух методов 
```
    String getAttributeOfType();
    String getTypeName();
```
3. Создать три абстрактных реализаций интерфейса. 
Например: `VehicleTypeByPurpose`, класс в котором будет переопределен метод `getAttributeOfType` и обязательно необходимо переопределить метод `equals` класса `Object`.
```
public abstract class VehicleTypeByPurpose implements VehicleType {
    private String attribute = "Vehicle type by purpose";

    @Override
    public String getAttributeOfType() {
        return attribute;
    }

     @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        VehicleTypeByPurpose that = (VehicleTypeByPurpose) o;

        return attribute != null ? attribute.equals(that.attribute) : that.attribute == null;
    }
}
```
Сделайте самостоятельно остальных два абстрактных класса `VehicleTypeByBodyTypes` и `VehicleTypeByFuelTypes`
4. Создадим имплементации каждого из абстрактных классов. В них необходимо переопределить только метод `getTypeName`
Например:
```
public class PassengerType extends VehicleTypeByPurpose {

    @Override
    public String getTypeName() {
        return CarTypeEnum.PASSENGER.name();
    }
}
```
и 
```
public class TruckType extends VehicleTypeByPurpose {

    @Override
    public String getTypeName() {
        return GenreEnum.TRUCK.name();
    }
}
```

Создайте остальные имлементации.

5. Также необходимо будет, используя композицию, добавить в класс `VehicleAd` четыре поля

```
import java.util.List;

public class VehicleAd {
    private String model;
    private String id;
    private VehicleTypeByPurpose vehicleTypeByPurpose;
    private VehicleTypeByBodyTypes vehicleTypeByBodyTypes;
    private VehicleTypeByFuelTypes vehicleTypeByFuelTypes;

    public VehicleAd(String model, String id, VehicleTypeByPurpose vehicleTypeByPurpose, 
    VehicleTypeByBodyTypes vehicleTypeByBodyTypes, VehicleTypeByFuelTypes vehicleTypeByFuelTypes) {
        this.model = model;
        this.id = id;
        this.vehicleTypeByPurpose = vehicleTypeByPurpose;
        this.vehicleTypeByBodyTypes = vehicleTypeByBodyTypes;
        this.vehicleTypeByFuelTypes = vehicleTypeByFuelTypes;
    }

    //for creating new Car
    public VehicleAd(String model) {
        this.model = model;
    }

    public VehicleTypeByPurpose getVehicleTypeByPurpose() {
            return vehicleTypeByPurpose;
    }
    
    public VehicleTypeByBodyTypes getVehicleTypeByBodyTypes() {
                return vehicleTypeByBodyTypes;
    }
    
    public VehicleTypeByFuelTypes getVehicleTypeByFuelTypes() {
                    return vehicleTypeByFuelTypes;
    }

    public String getModel() {
        return model;
    }

    public String getId() {
        return id;
    }

    public String toString(){
        return this.model;
    }
}
```

6. Пора создать сервис `AdsService`, в котором можно будет отфильтровать объявления по каждому типу 

```
import java.util.ArrayList;
import java.util.List;

public class AdsService {
    private List<VehicleAd> adList;

    public void setAdList(List<VehicleAd> adList) {
        this.adList = adList;
    }

    public void filterByVehicleTypeByPurpose(VehicleTypeByPurpose vehicleType) {
         for (VehicleAd ad : adList) {
            if (ad.getVehicleTypeByPurpose().equals(vehicleType)) {
              System.out.println("Объявление № " + ad.getId() + " подходит под данный фильтр с атрибутом: тип авто - " 
              + vehicleType.getTypeName() + ", атрибут фильтра " + vehicleType.getAttributeOfType());
            } else {
            System.out.println("Объявление № " + ad.getId() + " не прошло фильтр: тип авто - " + vehicleType.getTypeName() + ", критерий- " + 
                                            vehicleType.getAttributeOfType() + ", так как имеет тип авто " +
                                            ad.getVehicleTypeByPurpose().getTypeName());
            }
        }
  }
  
  public void filterByVehicleTypeByBodyTypes(VehicleTypeByBodyTypes vehicleType) {
           //TODO 
    }
    
  public void filterByVehicleTypeByFuelTypes(VehicleTypeByFuelTypes vehicleType) {
           //TODO 
    }
  
}
```

7. В классе Main.java необходимо будет создать несколько объектов класса `VehicleAd`, используя конструктор и убедиться, 
что функция фильтрации была реализованы верно. Например:

```
   AdsService adsService = new AdsService();
   VehicleAd ad = new VehicleAd("Volvo", "123", new PassengerType(), 
       null, null);
   VehicleAd ad = new VehicleAd("Kamaz", "45", new TruckType(), 
          null, null);
    
    adsService.setAdList(Arrays.asList(ad));
   
    adsService.filterByVehicleTypeByPurpose(new PassengerType());
   
    adsService.filterByVehicleTypeByPurpose(new TruckType());
    
    //TODO Создайте объявление с типами CAR, SEDAN, PETROL и отфильтруйте объявления с бензиновым топливом
           
           
```


## Инструкция по выполнению домашнего задания

1. Зарегистрируйтесь на сайте [Repl.IT](http://repl.it/).
2. Перейдите в раздел **my repls**.
3. Нажмите кнопку **Start coding now!**, если приступаете впервые, или **New Repl**, если у вас уже есть работы.
4. В списке языков выберите `Java`.
5. Код пишите в левой части окна, вместо строки `System.out.println("Hello world!");`.
6. Опирайтесь на принятый [стиль оформления кода](https://github.com/netology-code/codestyle/blob/master/java/README.md).
7. Посмотреть результат выполнения файла можно, нажав на кнопку **Run**. Результат появится в правой части окна.
8. После окончания работы нажмите кнопку **Share** и скопируйте ссылку из поля Share link.
9. В личном кабинете на сайте [netology.ru](http://netology.ru/) в поле комментария к домашней работе вставьте скопированную ссылку и отправьте работу на проверку.

*Никаких файлов прикреплять не нужно.*