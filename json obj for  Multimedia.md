1)	Parameter “obj” defined refer this document 
2)	Source manager just pass the JSON string to meter & just give “sub_type”, “author” & “title” to status bar to avoid message length over bind’s limit when “sub_type” is “hu_play_start”.
3)	If specified app Id (1st parameter) does not match current source Id, update info to meter will be ignored.
4)	Valid values of field “data” & “source” in JSON between DA and IC communication as below:<br>
0: invalid
1: USB music
2: USB video
3: BT music
4: Online music
5: FM
6: AM
7: Online radio
......

**1.	This packet is used by HU to notify IC that a new music is going to play.IC can use the information to pop up a tip bar.**<br>

```
{
"type":"multimedia",
"timestamp":11111111,
"sub_type":"hu_play_start",
"source":1,  
"playlist_name":"heavy metal music",   
"index":1,
"reason":1,      
"author":"Liu Huan",
"title":"One Night In Beijing",
"length":4500,
"cover_pic_type":1,
"offset":2,
"artwork":[12,3,3,54,5,5,6,6,7,7,7]
}
```

***Notes:***<br>
1.**source:** the source field is defined as follows:0(invalid)1(usb music) 2(usb video) 3(blue-tooth) 4(online music) 5(FM) 6(AM) 7(online radio)etc. <br>
2.**cover_pic_type:** the cover_pic_type is defined as follows:0(no cover pic) 1(bng) 2(bmp) 3(jpg).<br>
3.**index:** the index field is the identification of this music in the playlist. <br>
4.**playlist_name:** playlist field is used to indicate which playlist the music is belonged to. <br>
5.**artwork:** artwork is used to contain the cover image raw data. <br>
6.**reason:** The reason field is used to indicate the exact reason why HU switch to the pre/next song. 0(other) 1(user button(pre/next)).<br>

**2.	This packet is used by HU to update the playing status to IC,including the pause or playing status and current progress.**<br>
```
{
"type":"multimedia",
"timestamp":11111111,
"sub_type":"hu_play_ongoing",
"index":1,
"status":1,
"progress":5,
"lyrics":"I’m a big big girl"
}
```
***Notes:***<br>
1.**status:** the status field is defined as follows:0(stop) 1(play).
2.**progress:** progress is the current playing time passed by the millisecond.

**3.	This packet is used by HU to notify IC that the current media will be end.**
```
{
"type":"multimedia",
"timestamp":11111111,
"sub_type":"hu_play_end"
}
```
***Notes:***<br>
This packet is not so important when you have multiple music to play.But if there is actually has nothing to play,it will be better to send this packet to info the ending.<br>
There are some implicit rules as below:<br>
1.	If the current focused source has been changed,the current playing music will be end.<br>
2.	If the current focused source has been disconnected,the current playing music will be end.<br>

**4.	This packet is used by HU to notify IC the media source connecting status.**<br>
```
{
"type":"multimedia",
"timestamp":11111111,
"sub_type":"hu_connect_status",
"data":1
}
```
***Notes:***<br>
**data:** The data field is used to indicate the connect status of all the sources.Each of these occupies a corresponding bit of the integer.<br>
          For example,bit 0 is used to record the status of usb,and so on.If the bit is 0,it indicates the disconnected status,otherwise 1 means connected.<br>

**5.	This packet is used by HU to info the current focused media source.**<br>
```
{
"type":"multimedia",
"timestamp":11111111,
"sub_type":"hu_focus_status",
"data":0
}
```
***Notes:***<br>
**data:** The data field is used to define the focused source as follows:0(no device focus) 1(usb focused) 2(USB video focused)...and so no.<br>

**6.	This packet is used by IC to switch focused source.If the HU received this command and switch successfully,HU need to update the focused media source using packet No5.**<br>
```
{
"type":"multimedia",
"timestamp":11111111,
"sub_type":"ic_switch_focus",
"data":0
}
```
***Notes:***<br>
**data:** The data field is used to define the focused source as follows:0(no device focus) 1(usb focused) 2(usb video)...and so no.<br>

**7.	This is a header packet.This packet is used by HU to notify IC that the playlist of a source will be updated.This packet will be followed by Packet No8.**<br>
```
{
"type":"multimedia",
"timestamp":11111111,
"sub_type":"hu_playlist_update",
"source":0,
"playlist_name":"Lily’s favorite",
"reason":1,
"quantity":20,
"total":100
}
```
***Notes:***<br>
1.	Conditions:when playlist in HU has been changed,or IC send the request to acquire more items.<br>
2.	Normally HU need to send 20 records when switching playlist.If there is less than 20,subject to the actual quantity.<br>
3.**reason:**	The reason field is used to indicate the exact reason why HU update the playlist. 0(no meaning) 1(required by IC) 2(there is playlist switch happened).<br>
              When the reason is playlist switch,IC should clear the previous playlist record and receive the newest one.<br>
4.**quantity:**	The quantity field is used to indicate the amount of items that HU will send now. <br>
5.**total:**	The total field is used to indicate the items number of playlist.<br>

**8.	This packet is used by HU to send the elaborate information of playlist.It is following the packet No.7.**<br>
```
{
"type":"multimedia",
"timestamp":11111111,
"sub_type":"hu_playlist_info",
"source":0,
"playlist_name":"Classic music",
"sequence_id":5,
"index":56,
"author":"Liu Huan",
"title":"One Night In Beijing",
"length":4500,
"cover_pic_type":1,
"offset":1,
"artwork":[12,3,3,54,5,5,6,6,7,7,7]
}
```

***Notes:***<br>
This is the playlist content.It will be send one by one by HU.<br>
**sequence_id:** The sequence_id is used to indicate the packet sequence number.<br>
**index:** The index is the id of the music in the playlist.<br>

**9.	This packet is used by IC to send the command when user click ok button.**<br>
```
{
"type":"multimedia",
"timestamp":11111111,
"sub_type":"ic_user_button",
"button":0
}
```
***Notes:***<br>
1.**button:** button define:0(pause) 1(play)<br>

**10.	This packet is used by IC to notify HU that user wants to play the song he chose in IC.**<br>
```
{
"type":"multimedia",
"timestamp":11111111,
"sub_type":"ic_switch_music",
"source":0,
"index":20
}
```
***Notes:***<br>
This means user want to play the 20th in the playlist.If it is switched successfully,HU will update the play information using packet No1.<br>

**11.	This packet is used by IC to request to get more information in the current playlist.**<br>
```
{
"type":"multimedia",
"timestamp":11111111,
"sub_type":"ic_request_playlist",
"source":0,
"start_idx":20,
"quantity":10
}
```
***Notes:***<br>
This means the IC needs more playlist info starting from 20th and the total amount is defined by quantity.<br>

