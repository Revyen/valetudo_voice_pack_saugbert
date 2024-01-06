# valetudo_voice_pack_saugbert

This is a voice pack of all the Dreame D10S Plus messages repeated with the Fin-Voice of [IIElevenLabs](https://elevenlabs.io/) in german.

>**These are not 1:1 translations ("Driving back to base" translates to "Driving home", "Positioning" is more like "Where am I") take a listen for yourself if you find it funny**

This repo took heavy insperation from https://github.com/spikeygg/valetudo_voice_pack_arnold_standard

Install as follows:

1. Root
2. Install Valetudo
3. Ssh into the vacuum
4. Backup files with SCP and to make a list of all of them

```bash
scp -i key.id_rsa root@192.168.1.101:/audio/EN/* backup/
```

5. Download/create your .wav files, save as 0.wav 1.wav etc. https://github.com/Findus23/voice_pack_dreame/blob/main/sound_list.csv use this for reference which file is which sound to avoid listening to original files to find out

6. Normalize WAV and convert to OGG. I used WSL Ubuntu 20.04 on Win11 (install vorbis-tools, ffmpeg) 

>files a normalized in this repo, do this if you want to do it from the beginning

```bash
mkdir ogg
cp en-backup/* ogg/
filenames=("0" "1" "2" "3" "4" "5" "6" "7" "8" "9" "10" "11" "12" "13" "14" "17" "18" "19" "20" "21" "23" "24" "25" "26" "27" "28" "29" "30" "31" "34" "35" "36" "37" "38" "39" "40" "41" "42" "43" "44" "45" "46" "47" "48" "50" "52" "54" "55" "56" "57" "58" "61" "62" "63" "64" "65" "66" "67" "68" "71" "72" "75" "82" "83" "84" "85" "86" "87" "90" "101" "102" "110" "116" "117" "118" "126" "127" "128" "129" "130" "133" "134" "135" "136" "137" "138" "143" "144" "145" "146" "149" "150" "151" "152" "153" "162" "163" "164" "171" "172" "187" "200" "257" "258" "259" "260" "261" "262" "267" "269" "274" "293" "294")
for filename in ${filenames[@]}; do ffmpeg -i "${filename}.wav" -filter:a loudnorm=I=-14:LRA=1:dual_mono=true:tp=-1 "${filename}_n.wav" && oggenc "${filename}_n.wav" --output "${filename}.ogg" --bitrate 100 --resample 16000; done
mv *.ogg ogg/
cd ogg
tar czf ../saugbert_voice.tar.gz *.ogg
md5sum ../saugbert_voice.tar.gz
```

7. Serve files. I use (PowerShell/CMD, not WSL!!) https://dashboard.ngrok.com/get-started/setup) and python 3

```powershell
python -m http.server 8000 --bind 0.0.0.0
ngrock http 8000
```

8. Open ngrok url in web browser, right click saugbert_voice.tar.gz "copy link"

9. Go to Valetudo eg. http://192.168.1.101/#/robot/settings/misc language SAUGBERT, hash from md5sum command (was `6d458d668427ad1435a9b8ffaac3fe45` for me)

All audio files were created with [elevenlabs.io](https://elevenlabs.io/)