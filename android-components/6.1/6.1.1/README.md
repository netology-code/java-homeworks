Домашнее задание по лекции 6.1. - Компоненты View. Иерархия Views

==
## Задача 1. Взаимоисключающие CheckBox.
### Описание

В сценариях использования мобильных приложений постоянно присутствует сценарий выбора из 2х и более вариантов. Для обеспечения этой возможности- существует элемент CheckBox. 
Создайте простую форму для приема оплаты из следующих элементов:
*EditText для ввода денег
*EditText для ввода информации об оплате
*Кнопка OK для подтверждения оплаты
*Несколько взаимоисключающих CheckBox для выбора способа оплаты- “С банковской карты”, “С мобильного телефона”, “Наличными по адресу”.
*При выборе соответствующего CheckBox- на EditText с информацией об оплате накладывается соответствующая маска- только цифры, только номер телефона или любой текст. 
*При клике на кнопку “ОК”- выводим Toast с выбранной информацией


### Реализация

Из описания задачи видно, что в ресурснов файле activity_main.xml (папка layout) нужно использовать View типов EditText, Button и CheckBox. 
Для отображения требуемых View, нужно расположить их в ViewGroup -е. Выберем в качестве него LinearLayout, и установим вертикальную опцию для него.
В этом случае наш xml файл будет имееть следующий вид:
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

    <EditText
        android:id="@+id/inputMoney"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:hint="Ввод денег"
		android:inputType="number"/>

    <EditText
        android:id="@+id/inputInfo"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:hint="Информация об оплате" />

    <Button
        android:id="@+id/btnOK"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="OK" />

    <CheckBox
        android:id="@+id/bankCardChkBx"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="С банковской карты"/>

    <CheckBox
        android:id="@+id/mobilePhoneChkBx"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="С мобильного телефона"/>

    <CheckBox
        android:id="@+id/cashAddressChkBx"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Наличными по адресу"/>
</LinearLayout>
```  
Где все элементы обозначены соответствующими Id. Лучшей практикой является перенос текстов из атрибута android:text и android:hint в файл string.xml.

2. Слудеющим шагом будет инициализация элементов View в MainActivity. Инициализацию элементов осуществим в отдельно созданном методе initViews().
Инициализация элементов примет следующий вид:
```  
    private EditText mInputMoney;
    private EditText mInputInfo;
    private Button mBtnOk;
    private CheckBox mBankCardChkBx;
    private CheckBox mMobilePhoneChkBx;
    private CheckBox mCashAddressChkBx;
	
	@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initViews();
    }
	
   private void initViews(){
       mInputMoney = findViewById(R.id.inputMoney);
       mInputInfo = findViewById(R.id.inputInfo);
       mBtnOk = findViewById(R.id.btnOK);
       mBankCardChkBx = findViewById(R.id.bankCardChkBx);
       mMobilePhoneChkBx = findViewById(R.id.mobilePhoneChkBx);
       mCashAddressChkBx = findViewById(R.id.cashAddressChkBx);
   }
  ```   
3. Теперь нужно реализовать функцию взаимоисключаения для имеющихся CheckBox. Так как, при выборе однова из CheckBox-ов, остальные должны перейди в состояние "не выбран", 
то при активации одного из элементов, нужно сбросить значения для остальных, для этого напишем функцию resetCheckBoxes():
```  
    private void resetCheckBoxes(){
        mBankCardChkBx.setChecked(false);
        mMobilePhoneChkBx.setChecked(false);
        mCashAddressChkBx.setChecked(false);
    } 
```  
Нажатие элемента CheckBox можно зарегистрировать в методе *setOnCheckedChangeListener()*, с реализацией интерфейса *CompoundButton.OnCheckedChangeListener()*, после нужно осуществить переопределение метода *onCheckedChanged()*.
Так как у нас имеются несколько CheckBox-ов, то оптимально будет создать один интерфейс для всех и осуществить действия взависимости от элемента. 
Для этого создадим один интерфейс и зарегистрируем для все имеющихся элементов CheckBox, в этом случае метод initViews() примет следующий вид:
```  
  private void initViews() {
        mInputMoney = findViewById(R.id.inputMoney);
        mInputInfo = findViewById(R.id.inputInfo);
        mBtnOk = findViewById(R.id.btnOK);
        mBankCardChkBx = findViewById(R.id.bankCardChkBx);
        mMobilePhoneChkBx = findViewById(R.id.mobilePhoneChkBx);
        mCashAddressChkBx = findViewById(R.id.cashAddressChkBx);

        mBankCardChkBx.setOnCheckedChangeListener(checkedChangeListener);
        mMobilePhoneChkBx.setOnCheckedChangeListener(checkedChangeListener);
        mCashAddressChkBx.setOnCheckedChangeListener(checkedChangeListener);
    }
```  
  Где реализация checkedChangeListener интерфейса имеет следующий вид:
``` 
	CompoundButton.OnCheckedChangeListener checkedChangeListener = new CompoundButton.OnCheckedChangeListener() {
        @Override
        public void onCheckedChanged(CompoundButton compoundButton, boolean b) {
            if (b) {
                switch (compoundButton.getId()) {
                    case R.id.bankCardChkBx:
                        resetCheckBoxes();
                        mBankCardChkBx.setChecked(true);
                        break;
                    case R.id.mobilePhoneChkBx:
                        resetCheckBoxes();
                        mMobilePhoneChkBx.setChecked(true);
                        break;
                    case R.id.cashAddressChkBx:
                        resetCheckBoxes();
						mCashAddressChkBx.setChecked(true);
                        break;
                    default:
                }
            }
        }
    };
``` 
 Осталось написать для каждого элемента CheckBox действие выбора, и реализация функционала накладывания маски на соответствующий EditText.

4. Накладование маски означает что ввод данных в EditText ограничевается соответствующим значением.
 В xml накладование маски можно осуществить при помощи атрибута android:inputType (он уже добавлен для элемента EditText ввода денег), но нам нужно осуществить данный функционал программно, так как маска меняется в зависимости от выбранного CheckBox.
 Менять маску программно можно используя метод setInputType. В таком случае реализация интерфейса CompoundButton.OnCheckedChangeListener примет следующий вид:
 ``` 
 CompoundButton.OnCheckedChangeListener checkedChangeListener = new CompoundButton.OnCheckedChangeListener() {
        @Override
        public void onCheckedChanged(CompoundButton compoundButton, boolean b) {
            if (b) {
                switch (compoundButton.getId()) {
                    case R.id.bankCardChkBx:
                        resetCheckBoxes();
                        mBankCardChkBx.setChecked(true);
                        mInputInfo.setInputType(InputType.TYPE_CLASS_NUMBER);
                        break;
                    case R.id.mobilePhoneChkBx:
                        resetCheckBoxes();
                        mMobilePhoneChkBx.setChecked(true);
                        mInputInfo.setInputType(InputType.TYPE_CLASS_PHONE);
                        break;
                    case R.id.cashAddressChkBx:
                        resetCheckBoxes();
                        mInputInfo.setInputType(InputType.TYPE_CLASS_TEXT);
                        mCashAddressChkBx.setChecked(true);
                        break;
                    default:
                }
            }
        }
    };
``` 

Осталось обработать нажатие кнопки "ОК" и вывести сообщение на экран используя Toast.
Структура Toast-а имеет следующий вид:
 ``` 
 Toast.makeText(MainActivity.this, "необходимое сообщение", Toast.LENGTH_LONG).show();
``` 
