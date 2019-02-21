## Задача 3. CalendarView «Сроки задачи».
### Описание

При работе с мобильными приложениями приходится часто работать с выбором дат. 
Создайте мобильное приложение “Сроки задачи” со следующими элементами:
CalendarView для даты- дата-время старта задачи
CalendarView для даты- дата-время окончания задачи
Кнопку ОК- по клику на которую проверяем, чтобы дата старта была меньше даты окончания.
Если проверка пройдена- выводим Toast с выбранными датами. 
Если не пройдена- выводим в Toast сообщение об ошибке и очищаем поля ввода дат. 


### Реализация

1. Из описания задачи видно, что в ресурснов файле activity_main.xml (папка layout) нужно использовать View типов CalendarView и Button. 
Для отображения требуемых View, нужно расположить их в ViewGroup -е.Компоненты CalendarView и Button расположим в ViewGroup-е LinearLayout, и установим вертикальную опцию для него.
Скроим компоненты CalendarView при старте приложения (отобразим соответствующий CalendarView только после нажатия на кнопку выбора даты).
В этом случае наш xml файл будет имееть следующий вид:
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center_vertical"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <Button
		android:id="@+id/chooseStartDate"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="16dp"
        android:gravity="left|center_vertical"
        android:text="Дата-время старта задачи:" />

    <Button
		android:id="@+id/chooseEndDate"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="16dp"
        android:gravity="left|center_vertical"
        android:text="Дата-время окончания задачи:" />

    <CalendarView
        android:id="@+id/startDateCalendar"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="16dp"/>

    <CalendarView
        android:id="@+id/endtDateCalendar"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:background="#E2E2E2"/>

    <Button
        android:id="@+id/btnOK"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="16dp"
        android:text="OK" />

</LinearLayout>
```
Где все элементы обозначены соответствующими Id. Лучшей практикой является перенос текстов из атрибута android:text в файл string.xml.
Так же мы прописали фиксированные положения и цвет фона CalendarView для удобство пользователя.


2. Реализуем инициализацию элементов View в MainActivity в отдельно созданном методе initViews().
В этом случае инициализация элементов примет следующий вид:

    ```
	  private Button mChooseStartDate;
    private Button mChooseEndDate;
    private CalendarView mStartDateCalendar;
    private CalendarView mEndtDateCalendar;
    private Button mBtnOK;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initViews();
    }

    private void initViews() {
        mChooseStartDate = findViewById(R.id.chooseStartDate);
        mChooseEndDate = findViewById(R.id.chooseEndDate);
        mStartDateCalendar = findViewById(R.id.startDateCalendar);
        mEndtDateCalendar = findViewById(R.id.endtDateCalendar);
        mBtnOK = findViewById(R.id.btnOK);
		
		// Скроем календари при запуске приложения
		mStartDateCalendar.setVisibility(View.GONE);
        mEndtDateCalendar.setVisibility(View.GONE);
	
      }
	  ```
  
3.Отобразить календари нам помогут методы setOnClickListener соответствующих кнопок mChooseStartDate и mChooseEndDate:

  	mChooseStartDate.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                mStartDateCalendar.setVisibility(View.VISIBLE);
                mEndtDateCalendar.setVisibility(View.GONE);
            }
        });

        mChooseEndDate.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                mEndtDateCalendar.setVisibility(View.VISIBLE);
                mStartDateCalendar.setVisibility(View.GONE);
            }
        });

    
    
4.Выбор даты CalendarView можно зарегистрировать в методе *setOnDateChangeListener()*, с реализацией интерфейса *CalendarView.OnDateChangeListener()*, после нужно осуществить переопределение метода *onSelectedDayChange()*.
	Пропишем данный метод для обоих CalendarView:
  

		mStartDateCalendar.setOnDateChangeListener(new CalendarView.OnDateChangeListener() {
            @Override
            public void onSelectedDayChange(@NonNull CalendarView calendarView, int i, int i1, int i2) {

            }
        });

        mEndtDateCalendar.setOnDateChangeListener(new CalendarView.OnDateChangeListener() {
            @Override
            public void onSelectedDayChange(@NonNull CalendarView calendarView, int i, int i1, int i2) {

            }
        });

   
Где значения int i, int i1, int i2 соответствены значениям год, месяц, день.
После выбора даты нужно скрыть данных календарь и присвоить значения соответствующей кнопке,
так же внесем переменные для сохранения значений выбранных дат, в таком случае мы получим:
    
    
		private long mStartDate;
		private String mStartDateTxt;
		private long mEndDate;
		private String mEndDateTxt;
	
	      mStartDateCalendar.setOnDateChangeListener(new CalendarView.OnDateChangeListener() {
            @Override
            public void onSelectedDayChange(@NonNull CalendarView calendarView, int i, int i1, int i2) {
                mStartDateTxt = i+"-"+i1+"-"+i2;
                mChooseStartDate.setText("Дата-время старта задачи: " + mStartDateTxt);
                GregorianCalendar gregorianCalendar = new GregorianCalendar();
                gregorianCalendar.set(i, i1, i2);
                mStartDate = gregorianCalendar.getTimeInMillis();
                calendarView.setVisibility(View.GONE);
            }
        });

        mEndtDateCalendar.setOnDateChangeListener(new CalendarView.OnDateChangeListener() {
            @Override
            public void onSelectedDayChange(@NonNull CalendarView calendarView, int i, int i1, int i2) {
                mEndDateTxt = i+"-"+i1+"-"+i2;
                mChooseEndDate.setText("Дата-время окончания задачи: " + mEndDateTxt);
                GregorianCalendar gregorianCalendar = new GregorianCalendar();
                gregorianCalendar.set(i, i1, i2);
                mEndDate = gregorianCalendar.getTimeInMillis();
                calendarView.setVisibility(View.GONE);
            }
        });
		 

Где GregorianCalendar используется для того чтобы сохранить выбранную дату в милисекундах, для дальнейшей проверки.
	
5. Осталось реализовать нажатие кнопки "ОК" и показать Toast с проверкой дат:
 -	Если проверка пройдена- выводим Toast с выбранными датами. 
 -	Если не пройдена- выводим в Toast сообщение об ошибке и очищаем поля ввода дат. 

       
        mBtnOK.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                if (mStartDate > mEndDate){
                    Toast.makeText(MainActivity.this, "Ошибка", Toast.LENGTH_LONG).show();
                    mChooseStartDate.setText("Дата-время старта задачи:");
                    mChooseEndDate.setText("Дата-время окончания задачи:");
                } else {
                    Toast.makeText(MainActivity.this, "старт: " + mStartDateTxt + " окончаниe: " + mEndDateTxt, Toast.LENGTH_LONG).show();
                }
            }
        });
      
