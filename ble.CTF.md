# 1st 
Followed the instructions and got the flag.
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $(echo -n "12345678901234567890"|xxd -ps)
for reading the handle --> gatttool -b <MAC> --char-read -a 0x<handle>
# 2nd
We have to find the value stored in the handle 0x002e. so i used the command "gatttool -b EC:E3:34:1C:0D:D2 --char-read -a 0x002e". I got some hex values, I converted all the values into the text.
64 32 30 35 33 30 33 65 30 39 39 63 65 66 66 34 34 38 33 35 --> d205303e099ceff44835
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $(echo -n "d205303e099ceff44835"|xxd -ps)
# 3rd
In this i find the text value in the handle 0x0030 and that is 4d 44 35 20 6f 66 20 44 65 76 69 63 65 20 4e 61 6d 65. I converted it to the ASCII text that is "MD5 of Device Name".
I find the device name that is "BLECTF" convert into the MD5 that is 5cd56d74049ae40f442ece036c6f4f06.
Then i entered gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $ (echo -n "5cd56d74049ae40f442ece036c6f4f06"|xxd -ps) but it shows error because 
the string must be in 20 characters .
Then i entered "gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $ (echo -n "5cd56d74049ae40f442e"|xxd -ps)". 
# 4th
GATT (Generic Attribute Profile) defines how data is organized and exchanged between devices.
extra attributes are:
Device information (name, manufacturer, model)
Sensor data (temperature, heart rate, battery level)
Configuration and control values
gatttool -b EC:E3:34:1C:0D:D2 -I
then i entered connect, it replies Connection successful.
then I entered primary i showed 3 different handle 
0x1801 – Generic Attribute service
0x1800 – Generic Access service
0x00FF – Custom service
then i entered the gatttool -b EC:E3:34:1C:0D:D2 --characteristics
it returns 
handle = 0x0002, char properties = 0x20, char value handle = 0x0003, uuid = 00002a05-0000-1000-8000-00805f9b34fb
handle = 0x0015, char properties = 0x02, char value handle = 0x0016, uuid = 00002a00-0000-1000-8000-00805f9b34fb
handle = 0x0017, char properties = 0x02, char value handle = 0x0018, uuid = 00002a01-0000-1000-8000-00805f9b34fb
handle = 0x0019, char properties = 0x02, char value handle = 0x001a, uuid = 00002aa6-0000-1000-8000-00805f9b34fb
handle = 0x0029, char properties = 0x02, char value handle = 0x002a, uuid = 0000ff01-0000-1000-8000-00805f9b34fb
handle = 0x002b, char properties = 0x0a, char value handle = 0x002c, uuid = 0000ff02-0000-1000-8000-00805f9b34fb
handle = 0x002d, char properties = 0x02, char value handle = 0x002e, uuid = 0000ff03-0000-1000-8000-00805f9b34fb
....
Device Name is assigned UUID 0x2a00 
from the response , uuid = 00002a00
handle = 0x0015, char properties = 0x02, char value handle = 0x0016, uuid = 00002a00-0000-1000-8000-00805f9b34fb 
then read that handle 0x0016
gatttool -b EC:E3:34:1C:0D:D2 --char-read -a 0x0016
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $(echo -n "2b00042f7481c7b056c4"|xxd -ps)

# 5th
I find the value in the handle 0x0032 that is 57 72 69 74 65 20 61 6e 79 74 68 69 6e 67 20 68 65 72 65 --> Write anything here
I understand that we need to write into that handle. I converted the text into a hex value and i write into the handle by "gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x0032 -n 61 62 68 69 20 68 65 72 65". Then i read the value in that handle that is 33 38 37 33 63 30 32 37 30 37 36 33 35 36 38 63 66 37 61 61 -->3873c0270763568cf7aa
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $(echo -n "3873c0270763568cf7aa"|xxd -ps)

# 6th
I find the value in the handle 0x0034 that is 57 72 69 74 65 20 74 68 65 20 61 73 63 69 69 20 76 61 6c 75 65 20 22 79 6f 22 20 68 65 72 65 --> Write the ascii value "yo" here
I converted the text yo to hex "796F". Then i write it into the handle 0x0034 by "gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x0034 -n 796F". Then read the value of the handle 0x0034 that is 63 35 35 63 36 33 31 34 62 33 64 62 30 61 36 31 32 38 61 66 --> c55c6314b3db0a6128af
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $(echo -n "c55c6314b3db0a6128af"|xxd -ps)

# 7th
0x0036 --> Write the hex value 0x07 here
(Firstly i entered gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x0036 -n 7 but it showed invalid value) then i entered "gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x0036 -n 07". 
Then value of handle 0x0036 is 31 31 37 39 30 38 30 62 32 39 66 38 64 61 31 36 61 64 36 36 --> 1179080b29f8da16ad66
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $(echo -n "1179080b29f8da16ad66"|xxd -ps)

# 8th
gatttool -b EC:E3:34:1C:0D:D2 --char-read -a 0x0038 --> 57 72 69 74 65 20 30 78 43 39 20 74 6f 20 68 61 6e 64 6c 65 20 35 38 --> Write 0xC9 to handle 58
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x0058 -n C9 (XXX)
58--> 0x003A
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x003A -n C9
gatttool -b EC:E3:34:1C:0D:D2 --char-read -a 0x0038 --> 66 38 62 31 33 36 64 39 33 37 66 61 64 36 61 32 62 65 39 66 --> f8b136d937fad6a2be9f
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $(echo -n "f8b136d937fad6a2be9f"|xxd -ps)

# 9th
i created a file called hello.sh
entered the following code and save
"
#!/bin/bash

for i in {0..255}; do
    gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x3c -n $(printf "%02X" "$i")
    flag="$(gatttool -b EC:E3:34:1C:0D:D2 --char-read -a 0x3c | cut -d: -f2 | xxd -r -p)"
    echo "$flag"
done
"
run the code by -->bash hello.sh
got the flag as --> 933c1fcfa8ed52d2ec05

# 10th
Same like the previous one created a nano file called 10th.sh and entered 
"
#!/bin/bash

for i in {1..1000};do 
gatttool -b EC:E3:34:1C:0D:D2 --char-read -a 0x003e
done
"
then i run the file by "bash 10th.sh"
it returned 36 66 66 63 64 32 31 34 66 66 65 62 64 63 30 64 30 36 39 65 -->6ffcd214ffebdc0d069e
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $(echo -n "6ffcd214ffebdc0d069e"|xxd -ps)

# 11th
Keeps the connection open and waits for:
notifications
indications
gatttool -b EC:E3:34:1C:0D:D2 -a 0x40 --char-write-req -n 69 --listen
Notification handle = 0x0040 value: 35 65 63 33 37 37 32 62 63 64 30 30 63 66 30 36 64 38 65 62 --> 5ec3772bcd00cf06d8eb
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $(echo -n "5ec3772bcd00cf06d8eb"|xxd -ps)

# 12th
gatttool -b EC:E3:34:1C:0D:D2 --char-read -a 0x0042 --> 4c 69 73 74 65 6e 20 74 6f 20 68 61 6e 64 6c 65 20 30 78 30 30 34 34 20 66 6f 72 20 61 20 73 69 6e 67 6c 65 20 69 6e 64 69 63 61 74 69 6f 6e --> Listen to handle 0x0044 for a single indication
gatttool -b EC:E3:34:1C:0D:D2 -a 0x0044 --char-write-req -n 69 --listen -->63 37 62 38 36 64 64 31 32 31 38 34 38 63 37 37 63 31 31 33 -->c7b86dd121848c77c113
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $(echo -n "c7b86dd121848c77c113"|xxd -ps)

# 13th
gatttool -b EC:E3:34:1C:0D:D2 --char-read -a 0x0046 --> 4c 69 73 74 65 6e 20 74 6f 20 6d 65 20 66 6f 72 20 6d 75 6c 74 69 20 6e 6f 74 69 66 69 63 61 74 69 6f 6e 73 --> Listen to me for multi notifications
gatttool -b EC:E3:34:1C:0D:D2 -a 0x46 --char-write-req -n 69 --listen
Characteristic value was written successfully
Notification handle = 0x0046 value: 55 20 6e 6f 20 77 61 6e 74 20 74 68 69 73 20 6d 73 67 00 00 
Notification handle = 0x0046 value: 63 39 34 35 37 64 65 35 66 64 38 63 61 66 65 33 34 39 66 64 
Notification handle = 0x0046 value: 63 39 34 35 37 64 65 35 66 64 38 63 61 66 65 33 34 39 66 64 
Notification handle = 0x0046 value: 63 39 34 35 37 64 65 35 66 64 38 63 61 66 65 33 34 39 66 64 
...
55 20 6e 6f 20 77 61 6e 74 20 74 68 69 73 20 6d 73 67 00 00 63 39 34 35 37 64 65 35 66 64 38 63 61 66 65 33 34 39 66 64 --> U no want this msgc9457de5fd8cafe349fd
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $(echo -n "c9457de5fd8cafe349fd"|xxd -ps)

# 14th
There is a error in the ble ctf github there they provide the handle as 0x0042 but that handle we already opened and the correct handle is 0x0048.
gatttool -b EC:E3:34:1C:0D:D2 --char-read -a 0x0048 --> 4c 69 73 74 65 6e 20 74 6f 20 68 61 6e 64 6c 65 20 30 78 30 30 34 61 20 66 6f 72 20 6d 75 6c 74 69 20 69 6e 64 69 63 61 74 69 6f 6e 73 --> Listen to handle 0x004a for multi indications
gatttool -b EC:E3:34:1C:0D:D2 -a 0x004a --char-write-req -n 69 --listen 
Characteristic value was written successfully
Indication   handle = 0x004a value: 55 20 6e 6f 20 77 61 6e 74 20 74 68 69 73 20 6d 73 67 00 00 
Indication   handle = 0x004a value: 62 36 66 33 61 34 37 66 32 30 37 64 33 38 65 31 36 66 66 61 
Indication   handle = 0x004a value: 62 36 66 33 61 34 37 66 32 30 37 64 33 38 65 31 36 66 66 61 
Indication   handle = 0x004a value: 62 36 66 33 61 34 37 66 32 30 37 64 33 38 65 31 36 66 66 61 
Indication   handle = 0x004a value: 62 36 66 33 61 34 37 66 32 30 37 64 33 38 65 31 36 66 66 61 
Indication   handle = 0x004a value: 62 36 66 33 61 34 37 66 32 30 37 64 33 38 65 31 36 66 66 61 
....
55 20 6e 6f 20 77 61 6e 74 20 74 68 69 73 20 6d 73 67 00 00 62 36 66 33 61 34 37 66 32 30 37 64 33 38 65 31 36 66 66 61 --> U no want this msgb6f3a47f207d38e16ffa
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $(echo -n "b6f3a47f207d38e16ffa"|xxd -ps)

# 16th
MTU 444 means: Maximum Transmission Unit = 444 bytes for a BLE (Bluetooth Low Energy) connection. Used for To send and receive much larger data in a single BLE operation
gatttool -b EC:E3:34:1C:0D:D2 -I
then connect to that specific mac addrress 
mtu 444
MTU was exchanged successfully: 444
then i read the char value in the handle 0x004e (char-read-hnd 0x4e)
62 31 65 34 30 39 65 35 61 34 65 61 66 39 66 65 35 31 35 38 --> b1e409e5a4eaf9fe5158
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $(echo -n "b1e409e5a4eaf9fe5158"|xxd -ps)

# 17th
gatttool -b EC:E3:34:1C:0D:D2 --mtu 444 --char-read -a 0x0050--> 57 72 69 74 65 2b 72 65 73 70 20 27 68 65 6c 6c 6f 27 20 20 --> Write+resp 'hello' 
We have to write hello 
hello --> 68656c6c6f
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x50 -n 68656c6c6f --listen 
gatttool -b EC:E3:34:1C:0D:D2 --char-read -a 0x0050 --> 64 34 31 64 38 63 64 39 38 66 30 30 62 32 30 34 65 39 38 30 00 --> d41d8cd98f00b204e980
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $(echo -n "d41d8cd98f00b204e980"|xxd -ps)

# 18th
Just like the previous one just write anything and listen to the handle 
gatttool -b EC:E3:34:1C:0D:D2 --char-read -a 0x0052 --> 4e 6f 20 6e 6f 74 69 66 69 63 61 74 69 6f 6e 73 20 68 65 72 65 21 20 72 65 61 6c 6c 79 3f --> No notifications here! really?
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x52 -n 68656c6c6f 
listen Characteristic value was written successfully
Notification handle = 0x0052 value: 66 63 39 32 30 63 36 38 62 36 30 30 36 31 36 39 34 37 37 62 --> fc920c68b6006169477b
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $(echo -n "fc920c68b6006169477b"|xxd -ps)

# 19th
I just go through all the properties that i used in this tasks.
Firstly i tried to listen notification/indication the handle .
gatttool -b EC:E3:34:1C:0D:D2 -a 0x0054 --char-write-req -n 69 --listen
Characteristic value was written successfully
Notification handle = 0x0054 value: 30 37 65 34 61 30 63 63 34 38 
Then i write something into the handle and read the char value in the handle 
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x0054 -n C9
gatttool -b EC:E3:34:1C:0D:D2 --char-read -a 0x0054
Characteristic value/descriptor: 66 62 62 39 36 36 39 35 38 66 
Then i just combined all the parts of the flag --> 30 37 65 34 61 30 63 63 34 38 66 62 62 39 36 36 39 35 38 66 --> 07e4a0cc48fbb966958f
66 62 62 39 36 36 39 35 38 66 30 37 65 34 61 30 63 63 34 38 --> fbb966958f07e4a0cc48 (correct flag)
I tried both of the but one of them is a wrong flag.
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $(echo -n "fbb966958f07e4a0cc48"|xxd -ps)

# 20th   
Hint is in the handle 0x56
gatttool -b EC:E3:34:1C:0D:D2 --char-read -a 0x0056 --> 6d 64 35 20 6f 66 20 61 75 74 68 6f 72 27 73 20 74 77 69 74 74 65 72 20 68 61 6e 64 6c 65 --> md5 of author's twitter handle
author's twitter handle --> hackgnar --> fe40eb2449bda7f9a997331ac09424e7 (md5) --> fe40eb2449bda7f9a997(20 char)
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $(echo -n "fe40eb2449bda7f9a997"|xxd -ps)
but this was a wrong flag 
the twitter handle original--> @hackgnar(not hackgnar) --> d953bfb9846acc2e15eecd5b467a79aa(md5) --> d953bfb9846acc2e15ee
gatttool -b EC:E3:34:1C:0D:D2 --char-write-req -a 0x002c -n $(echo -n "d953bfb9846acc2e15ee"|xxd -ps)

xxd is a Linux tool --> Converts data between binary to hex
-p --> Plain hex
