## Задача 2. Spinner «Страны-города-улицы».
### Описание

Еще более частый сценарий выбора в мобильном приложении- это работа с выпадающим списком. Чаще всего это происходит при работе с адресами.
Создайте форму выбора адреса доставки со следующими элементами:
Spinner со списком стран “Россия, Украина, Белоруссия”
Spinner со списком городов, в котором 
Если ранее выбрана Россия, то города- Москва, Питер, Казань
Если ранее выбрана Украина, то города- Киев, Одесса, Харьков
Если ранее выбрана Белоруссия, то города- Минск, Гомель, Торжок
Spinner с номерами домов от 1 до 50
Кнопкой ОК- по клику на которую показываем Toast с выбранными опциями
,
### Реализация
1. Для решения данной задачи создадим форму xml (с элементами Spinner и Button) и пропишем данные в виде массивов в string.xml  (тэг string-array)
```  
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center_vertical"
    android:orientation="vertical"
    android:padding="50dp"
    tools:context=".MainActivity">

    <Spinner
        android:id="@+id/countriesSpinner"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

    <Spinner
        android:id="@+id/citiesSpinner"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

    <Spinner
        android:id="@+id/houseNumberSpinner"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

    <Button
        android:id="@+id/showAddress"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Показать адрес"/>
</LinearLayout>

<resources>
    .....

    <string-array name="countries">
        <item>Россия</item>
        <item>Украина</item>
        <item>Белоруссия</item>
    </string-array>

    <string-array name="r_cities">
        <item>Москва</item>
        <item>Питер</item>
        <item>Казань</item>
    </string-array>

    <string-array name="u_cities">
        <item>Киев</item>
        <item>Одесса</item>
        <item>Харьков</item>
    </string-array>

    <string-array name="b_cities">
        <item>Минск</item>
        <item>Гомель</item>
        <item>Торжок</item>
    </string-array>

</resources>
```  

Где все элементы обозначены соответствующими Id. Лучшей практикой является перенос текстов из атрибута android:text в файл string.xml.

2. Слудеющим шагом будет инициализация элементов View в MainActivity. Инициализацию элементов осуществим в отдельно созданном методе initViews().
Инициализация элементов примет следующий вид:
```   
	private Spinner mCountriesSpinner;
    private Spinner mCitiesSpinner;
    private Spinner mHouseNumberSpinner;
    private Button mShowAddressBtn;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initViews();
    }

    private void initViews() {
        mCountriesSpinner = findViewById(R.id.countriesSpinner);
        mCitiesSpinner = findViewById(R.id.citiesSpinner);
        mHouseNumberSpinner = findViewById(R.id.houseNumberSpinner);
        mShowAddressBtn = findViewById(R.id.showAddressBtn);
		initSpinnerCountries();
		initHousNumbersSpinner();
    }
```  

Где в методе initHousNumbersSpinner() реализуем инициализацию Spinner-а номера домов:
	
3. Создадим переменную массива целестного числа Integer с размером 50
```   
   Integer[] houseNumbers = new Integer[50];
```   
  
	При помощи цикла присвоим значения для каждой ячейки массива houseNumbers:
  
```   
      for (int i = 1; i <= 50; i++) {
            houseNumbers[i - 1] = i;
        }
```    
  
	В конце присвоим полученные значения Spinner-у mHouseNumberSpinner. 
	В таком случае метод initHousNumbersSpinner() примет следующий вид:
  
```  
    private void initHousNumbersSpinner() {
        Integer[] houseNumbers = new Integer[50];
        for (int i = 1; i <= 50; i++) {
            houseNumbers[i - 1] = i;
        }
        ArrayAdapter<Integer> adapter = new ArrayAdapter<Integer>(this, android.R.layout.simple_spinner_item, houseNumbers);
        mHouseNumberSpinner.setAdapter(adapter);
    }
```  
	
4.  В методе initSpinnerCountries() реализуем присвоение данных и соответствующий функционал выбора для Spinner - а стран:
	 
```    
	private void initSpinnerCountries() {
        ArrayAdapter<CharSequence> adapterCountries = ArrayAdapter.createFromResource(this, R.array.countries, android.R.layout.simple_spinner_item);
        adapterCountries.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
        mCountriesSpinner.setAdapter(adapterCountries);
    }
```   
	Выбор одного из значений mCountriesSpinner можно зарегистрировать в методе *setOnItemSelectedListener()*, с реализацией интерфейса *AdapterView.OnItemSelectedListener()*, после нужно осуществить переопределение метода *onItemSelected()*.
	Где в зависимости от выбранной страно нужно осуществить инициализацию mCitiesSpinner-а.
	В таком случае метод initSpinnerCountries() примет следующий вид:
  
```    
	private void initSpinnerCountries() {
        ArrayAdapter<CharSequence> adapterCountries = ArrayAdapter.createFromResource(this, R.array.countries, android.R.layout.simple_spinner_item);
        adapterCountries.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
        mCountriesSpinner.setAdapter(adapterCountries);
 
		mCountriesSpinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> adapterView, View view, int i, long l) {
                String[] countries = getResources().getStringArray(R.array.countries);
                initSpinnerCities(countries[i]);
            }

            @Override
            public void onNothingSelected(AdapterView<?> adapterView) {

            }
        });
	}
```  

5. Теперь осуществим инициализацию mCitiesSpinner в зависимости от выбранной сраны. 
	Для этого нужно создать соответствующий адаптер в зависимости от выбранной страны и уже после присвоить данный адаптер mCitiesSpinner-у:
	
```   
    private void initSpinnerCities(String country) {
        ArrayAdapter<CharSequence> adapter = null;
        switch (country) {
            case "Россия":
                adapter = ArrayAdapter.createFromResource(this, R.array.r_cities, android.R.layout.simple_spinner_item);
                break;
            case "Украина":
                adapter = ArrayAdapter.createFromResource(this, R.array.u_cities, android.R.layout.simple_spinner_item);
                break;
            case "Белоруссия":
                adapter = ArrayAdapter.createFromResource(this, R.array.b_cities, android.R.layout.simple_spinner_item);
                break;
        }
        if (adapter != null) {
            adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
            mCitiesSpinner.setAdapter(adapter);
        }
    }
```  

6. Осталось отобразить выбранные опции на экране при нажатии кнопки. Структура Toast будет иметь следующий вид:

```  
        Toast.makeText(MainActivity.this
                        ,mCountriesSpinner.getSelectedItem().toString()
                                + " "
                                + mCitiesSpinner.getSelectedItem().toString()
                                + " "
                                + mHouseNumberSpinner.getSelectedItem().toString()
                        ,Toast.LENGTH_LONG)
						.show();  
```   
