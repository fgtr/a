### ubuntu 倒數計時器 KTimer + SOX

在 ubuntu 裝一個倒數計時器（Loop Timer )，可以提醒自已在電腦前坐多久，要起來活動一下。
先安裝 KTimer 
`sudo apt-get install ktimer`
啟動 KTimer 後你會發現，需在指令列輸入播音效的指令。
需再安裝可用指令播放的程式 SOX
`sudo apt-get install sox` 
想播 mp3 再安裝
`sudo apt-get install libsox-fmt-mp3`
如果要把所有支援的音頻都加入
sudo apt-get install libsox-fmt-all 
我設了個 sh 檔如下，所以指令列就填入檔案的位置：
/home/sunny/bin/playsong1.sh

```
#!/bin/bash
play -v 0.8 /home/sunny/songeffect/musical011.wav
```


-v 0.8 音量 0.8
enjoy!
什麼！音效檔從哪裡來？這[網站](http://www.grsites.com/archive/sounds/) 或 [more...](http://www.orangefreesounds.com/) 可參考。



> http://likesunniday.blogspot.com/2016/03/ubuntu-ktimer-sox.html
> 2017-06-27 16:06:52