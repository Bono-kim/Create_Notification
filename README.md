# Create_Notification
## 알림바 만들기
<img width="500" height="300" src="https://github.com/Bono-kim/Create_Notification/blob/master/15.PNG"/>
●add a button for the notification function.
<br>
알림기능에 사용할 버튼을 추가합니다.
<hr><br>
<img width="700" height="300" src="https://github.com/Bono-kim/Create_Notification/blob/master/16.PNG"/>
●First create a notification builder,I added an attribute.
<br>
먼저 알림 빌더를 생성하고,속성을 추가해줬다.
<hr><br>
<img width="700" height="180" src="https://github.com/Bono-kim/Create_Notification/blob/master/17.PNG"/>
●Added an action that occurs when the notification button is pressed.
<br>
알림버튼을 누를경우 생기는 액션을 추가했다.
<hr><br>
<img width="700" height="500" src="https://github.com/Bono-kim/Create_Notification/blob/master/18.PNG"/>
●Starting with the Oreo version, you need to create a channel to use the notification function.So I added a channel with the if statement.
<br>
오레오 버전부터는 알림기능을 사용하려면 채널을 만들어 줘야한다.그래서 if문으로 채널을 추가해줬다.
<hr><br>
<img width="200" height="300" src="https://github.com/Bono-kim/Create_Notification/blob/master/13.jpg"/>
●Application execution screen.
<br>
어플리케이션 실행화면이다.
<hr><br>
<img width="200" height="300" src="https://github.com/Bono-kim/Create_Notification/blob/master/15.jpg"/>
●Press a button to get a notification.
<br>
버튼을 누르면 알림이 뜬다.
<hr><br>
<img width="200" height="300" src="https://github.com/Bono-kim/Create_Notification/blob/master/14.jpg"/>
●Press notification,The site you designated as the Intent opens
<br>
알림을 누르면 인텐트로 지정해준 사이트로 이동했다.
<hr><br>

```python
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        findViewById(R.id.button1).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse("http://www.google.com/"));
                PendingIntent pendingIntent = PendingIntent.getActivity(MainActivity.this,
                        0,intent,
                        PendingIntent.FLAG_UPDATE_CURRENT);
                // 오레오 버전부터는 알림기능을 사용하려면채널이 존재 해야한다.
                // 이전 버전에서는 채널을 만들 필요가 없다.
                if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
                    Toast.makeText(getApplicationContext(), "오레오이상", Toast.LENGTH_SHORT).show();

                    int importance = NotificationManager.IMPORTANCE_HIGH;
                    String Noti_Channel_ID = "Noti";
                    String Noti_Channel_Group_ID = "Noti_Group";

                    NotificationManager notificationManager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
                    NotificationChannel notificationChannel = new NotificationChannel(Noti_Channel_ID, Noti_Channel_Group_ID, importance);

                    //채널이 있는지 체크해서 없을경우 만들고 있으면 채널을 재사용합니다.

                    if (notificationManager.getNotificationChannel(Noti_Channel_ID) != null) {
                        Toast.makeText(getApplicationContext(), "채널이 이미 존재합니다.", Toast.LENGTH_SHORT).show();
                    } else {
                        Toast.makeText(getApplicationContext(), "채널이 없어서 만듭니다.", Toast.LENGTH_SHORT).show();
                        notificationManager.createNotificationChannel(notificationChannel);
                    }
                    notificationManager.createNotificationChannel(notificationChannel);
                    //먼저 알림 빌더를 생성한다.
                    NotificationCompat.Builder builder = new NotificationCompat.Builder(getApplicationContext(),Noti_Channel_ID)
                            .setLargeIcon(null).setSmallIcon(R.drawable.ic_launcher_background)
                            .setWhen(System.currentTimeMillis()).setShowWhen(true)
                            .setAutoCancel(true).setPriority(NotificationCompat.PRIORITY_MAX)
                            .setContentTitle("알림 테스트")
                            .setDefaults(Notification.DEFAULT_SOUND)
                            .setContentText("알림 테스트.")
                            .setContentIntent(pendingIntent);
                    //알림바에 알림을 표시한다.
                    notificationManager.notify(0, builder.build());

                }
            }
        });
    }
}
```
