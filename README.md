# Ex.No:3 Develop a simple application to play and control the audio file in android studio.


## AIM:

To develop a simple application, to play and control the audio file and to perfrom the start,pause and stop opeartion in Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min.required Artic Fox)

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as audiofile and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml and create start,pause and stop button.

Step 6: Display message give in MainActivity file.

Step 7: Save and run the application.

## PROGRAM:
```
/*
Program to play and control the audio file”.
Developed by: Sanjit A
Registeration Number : 212224220087
*/
```

## activity_main.xml:
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center"
    android:padding="16dp"
    android:background="#121212">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/audio_player"
        android:textSize="28sp"
        android:textStyle="bold"
        android:textColor="#FFFFFF"
        android:layout_marginBottom="40dp"/>

    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        style="?android:attr/buttonBarStyle">

        <Button
            android:id="@+id/btnPlay"
            android:layout_width="100dp"
            android:layout_height="wrap_content"
            android:text="@string/play"
            android:layout_margin="8dp"
            style="?android:attr/buttonBarButtonStyle"/>

        <Button
            android:id="@+id/btnPause"
            android:layout_width="100dp"
            android:layout_height="wrap_content"
            android:text="@string/pause"
            android:layout_margin="8dp"
            style="?android:attr/buttonBarButtonStyle"/>

        <Button
            android:id="@+id/btnStop"
            android:layout_width="100dp"
            android:layout_height="wrap_content"
            android:text="@string/stop"
            android:layout_margin="8dp"
            style="?android:attr/buttonBarButtonStyle"/>
    </LinearLayout>

</LinearLayout>

```

## MainActivity.java:
```
package com.example.pmdex3;

import android.content.Context;
import android.content.res.AssetFileDescriptor;
import android.media.AudioAttributes;
import android.media.AudioManager;
import android.media.MediaPlayer;
import android.os.Bundle;
import android.util.Log;
import android.widget.Button;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private static final String TAG = "MainActivity";
    private MediaPlayer mediaPlayer;
    private boolean isPrepared = false;
    private boolean playRequested = false;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button btnPlay = findViewById(R.id.btnPlay);
        Button btnPause = findViewById(R.id.btnPause);
        Button btnStop = findViewById(R.id.btnStop);

        initMediaPlayer();

        // Play button
        btnPlay.setOnClickListener(v -> {
            if (mediaPlayer != null) {
                if (isPrepared) {
                    if (!mediaPlayer.isPlaying()) {
                        startPlayback();
                    }
                } else {
                    playRequested = true;
                }
            }
        });

        // Pause button
        btnPause.setOnClickListener(v -> {
            playRequested = false;
            if (mediaPlayer != null && mediaPlayer.isPlaying()) {
                mediaPlayer.pause();
            }
        });

        // Stop button
        btnStop.setOnClickListener(v -> {
            playRequested = false;
            if (mediaPlayer != null) {
                try {
                    if (isPrepared) {
                        mediaPlayer.stop();
                    }
                } catch (Exception e) {
                    Log.e(TAG, "Error stopping media player", e);
                }
                isPrepared = false;
                initMediaPlayer();
            }
        });
    }

    private void startPlayback() {
        // Ensure media stream volume is set to max
        AudioManager audioManager = (AudioManager) getSystemService(Context.AUDIO_SERVICE);
        if (audioManager != null) {
            audioManager.setStreamVolume(
                    AudioManager.STREAM_MUSIC,
                    audioManager.getStreamMaxVolume(AudioManager.STREAM_MUSIC),
                    0
            );
        }
        mediaPlayer.setVolume(1.0f, 1.0f);
        mediaPlayer.start();
    }

    private void initMediaPlayer() {
        if (mediaPlayer != null) {
            mediaPlayer.release();
        }
        mediaPlayer = new MediaPlayer();
        mediaPlayer.setAudioAttributes(
                new AudioAttributes.Builder()
                        .setContentType(AudioAttributes.CONTENT_TYPE_MUSIC)
                        .setUsage(AudioAttributes.USAGE_MEDIA)
                        .build()
        );
        try {
            AssetFileDescriptor afd = getResources().openRawResourceFd(R.raw.audio);
            mediaPlayer.setDataSource(afd.getFileDescriptor(), afd.getStartOffset(), afd.getLength());
            afd.close();
            mediaPlayer.prepareAsync();
            mediaPlayer.setOnPreparedListener(mp -> {
                isPrepared = true;
                if (playRequested) {
                    playRequested = false;
                    startPlayback();
                }
            });
            mediaPlayer.setOnCompletionListener(mp -> mp.seekTo(0));
        } catch (Exception e) {
            Log.e(TAG, "Error preparing media player", e);
        }
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        if (mediaPlayer != null) {
            mediaPlayer.release();
            mediaPlayer = null;
        }
    }
}

```

## OUTPUT
<img width="1917" height="1135" alt="image" src="https://github.com/user-attachments/assets/13296339-0156-4330-b599-25f24e68097e" />




## RESULT

Thus a simple application, to play and control the audio file and to perfrom the start,pause and stop opeartion in Android Studio is developed and executed successfully.
