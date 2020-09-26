# Week2

개별 스레드 공부 - 스탑워치 동작시켜보기(시작,일시정지,초기화버튼)
</br>

## 스레드 관계

- sub thread -> handler -> main thread

</br>

MainActivity.java
```java
public class MainActivity extends AppCompatActivity {

    private TextView txt_time;
    private Button btn_StartandStop,btn_refresh;
    private Thread timeThread = null; // null로 초기화
    private Boolean isRunning = true; //while문 쓰기 위해 작성

    private int i=0;
    private int buttonChange =0; //0 is Start // 1 is Stop
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        btn_StartandStop = (Button) findViewById(R.id.btn_StartandStop);
        btn_refresh = (Button) findViewById(R.id.btn_refresh);
        txt_time = (TextView) findViewById(R.id.txt_time);
        btn_StartandStop.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
             if(buttonChange==0) {
                isRunning = true;
                timeThread = new Thread(new timeThread());
                timeThread.start();
                btn_StartandStop.setText("Stop");
                buttonChange++;
             }
             else{
                 isRunning=false;
                 btn_StartandStop.setText("Start");
                 buttonChange--;
             }
            }
        });


        btn_refresh.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                if(buttonChange==0){
                    i=0;
                    txt_time.setText("00:00:00:00");
                }
            }
        });
    }

    Handler handler = new Handler(){
        public void handleMessage(Message msg){
            int mSec = msg.arg1 % 100;
            int sec = (msg.arg1 / 100) % 60;
            int min = (msg.arg1 / 100) / 60;
            int hour = (msg.arg1 / 100) /360;

            String result = String.format("%02d:%02d:%02d:%02d", hour, min, sec, mSec);
            txt_time.setText(result);

        }
    };

    public class timeThread implements  Runnable{
        public void run(){
            while(isRunning){
                try{
                    Thread.sleep(10);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                Message msg = new Message();
                msg.arg1 = i++;
            }
        }
    }

}
```


![3](https://user-images.githubusercontent.com/40445477/94340851-e59ad000-003f-11eb-9e78-cb1019192d14.PNG)


</br>

![4](https://user-images.githubusercontent.com/40445477/94340855-ee8ba180-003f-11eb-823d-ec77121f96fa.PNG)

