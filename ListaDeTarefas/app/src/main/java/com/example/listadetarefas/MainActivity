package com.example.listadetarefas

import android.annotation.SuppressLint
import android.content.Intent
import android.media.AudioDeviceCallback
import android.media.AudioDeviceInfo
import android.media.AudioManager
import android.os.Bundle
import android.provider.Settings
import androidx.appcompat.app.AppCompatActivity
import android.widget.ArrayAdapter
import android.widget.Button
import android.widget.EditText
import android.widget.ListView


class MainActivity : AppCompatActivity() {
        private lateinit var audioManager: AudioManager
        private var audioDeviceCallback: AudioDeviceCallback? = null

        @SuppressLint("MissingInflatedId", "WearRecents")
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)


            val listView = findViewById<ListView>(R.id.list_view)
            val button = findViewById<Button>(R.id.button)
            val editText = findViewById<EditText>(R.id.edit_text)
            val tasks = mutableListOf<String>()
            val adapter = ArrayAdapter(this, android.R.layout.simple_list_item_1, tasks)
            listView.adapter = adapter

            button.setOnClickListener {
                val task = editText.text.toString()
                if (task.isNotEmpty()) {
                    tasks.add(task)
                    adapter.notifyDataSetChanged()
                    editText.text.clear()
                }
            }

            audioManager = getSystemService(AUDIO_SERVICE) as AudioManager

            audioDeviceCallback = object : AudioDeviceCallback() {
                override fun onAudioDevicesAdded(addedDevices: Array<out AudioDeviceInfo>?) {

                }

                override fun onAudioDevicesRemoved(removedDevices: Array<out AudioDeviceInfo>?) {

                }
            }
            audioManager.registerAudioDeviceCallback(
                audioDeviceCallback!!,
                null
            )

            val intent = Intent(Settings.ACTION_BLUETOOTH_SETTINGS).apply {
                addFlags(Intent.FLAG_ACTIVITY_NEW_TASK or Intent.FLAG_ACTIVITY_CLEAR_TASK)
                putExtra("EXTRA_CONNECTION_ONLY", true)
                putExtra("EXTRA_CLOSE_ON_CONNECT", true)
                putExtra("android.bluetooth.devicepicker.extra.FILTER_TYPE", 1)
            }
            startActivity(intent)
        }

        override fun onDestroy() {
            super.onDestroy()
            audioDeviceCallback?.let {
                audioManager.unregisterAudioDeviceCallback(it)
            }
        }
}