## Задача 3. Иерархия "Автомобили"

### Описание
На этот раз рассмотрим предметную область "Автомобили".
Необходимо спроектировать приложение по размещению объявлений о продаже автомобилей. 

Реализуйте иерархию классов автомобилей с помощью наследования. В ней должны быть представлены три группы классов: 

→ по назначению (грузовые/легковые/пассажирские);

→ по типу кузова (седан/универсал/грузовик/автобус);

→ по типу топлива (бензиновые, дизельные, гибридные и электрические).

Необходимо с помощью наследования реализовать программу, в которой будет один базовый класс `VehicleType`, три наследника базового класса  
(`VehicleTypeByPurpose`, `VehicleTypeByBodyTypes`, `VehicleTypeByFuelTypes`), в которых будeт определено значение поля `attribute` каждой группы типов, 
и на каждый класс групп типов — по несколько классов, в которых будет определено конкретное определение типа.

Данный функционал пригодится в случае массовой фильтрации объявлений по какому-то искомому типу.

Также необходимо описать класс `VehicleAd`:
* с базовым набором полей, состоящим из id объявления, model (модели авто) и трех полей с каждым типом, 
* и `AdsService`, в котором будет осуществляться фильтрация объявлений.

### Процесс реализации
1. Создайте Enum класс `VehicleTypeEnum` с 8 возможными типами авто в нашей программе.
```
public enum VehicleTypeEnum {
    TRUCK,CAR,PASSENGER,SEDAN,WAGON,PICKUP,BUS,PETROL,DIESEL,HYBRID,ELECTRIC
}
```
2. Создайте класс `VehicleType` с protected полем `attribute` типа String, конструктором, принимающим один аргумент, и двумя методами.
```
    public String getAttributeOfType() {
          return attribute;
    }
    public String getTypeName() {
          return "Some vehicle type name";
    }
```
3. Создайте трёх наследников класса `VehicleType`. 
Например: `VehicleTypeByPurpose`, класс с конструктором, вызывающий конструктор предка. Обязательно переопределите метод `equals` класса `Object`.

```
public class VehicleTypeByPurpose extends VehicleType {
    public VehicleTypeByPurpose() {
            super("Vehicle type by purpose");
    }
    
    @Override
    public boolean equals(Object object) {
        if (object == null || getClass() != object.getClass()) return false;
    
         VehicleTypeByPurpose that = (VehicleTypeByPurpose) object;
         return attribute != null ? attribute.equals(that.attribute) : false;
     }
}
```
Сделайте самостоятельно остальные два класса: `VehicleTypeByBodyTypes` и `VehicleTypeByFuelTypes` с собственным значением поля `attribute`, присущим данной группе типов автомобилей.
4. Создайте наследников каждого из классов групп типов. В них необходимо переопределить только метод `getTypeName`.

Например:
```
public class PassengerType extends VehicleTypeByPurpose {

    @Override
    public String getTypeName() {
        return VehicleTypeEnum.PASSENGER.name();
    }
}
```
и 
```
public class TruckType extends VehicleTypeByPurpose {

    @Override
    public String getTypeName() {
        return VehicleTypeEnum.TRUCK.name();
    }
}
```

Создайте остальные имплементации.

5. Добавьте в класс `VehicleAd` пять полей.

```
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

6. Создайте сервис `AdsService`, в котором можно будет отфильтровать объявления по каждому типу. 

```
public class AdsService {
    private VehicleAd[] adList;

    public void setAdList(VehicleAd[] adList) {
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

7. В классе Main.java создайте несколько объектов класса `VehicleAd`, используя конструктор, и убедитесь, 
что функция фильтрации была реализована верно. 
Например:

```
    AdsService adsService = new AdsService();
    VehicleAd volvoAd = new VehicleAd("Volvo", "123", new PassengerType(), 
       new SedanType(), new PetrolType());
    VehicleAd kamazAd = new VehicleAd("Kamaz", "45", new TruckType(), 
          new PickupType(), new DieselType());
    
    adsService.setAdList(new VehicleAd[] {volvoAd, kamazAd});
   
    adsService.filterByVehicleTypeByPurpose(new PassengerType());
   
    adsService.filterByVehicleTypeByPurpose(new TruckType());
    
    //TODO Создайте объявление с типами CAR, SEDAN, PETROL и отфильтруйте объявления с бензиновым топливом
                  
```
