For this task I downloaded the GNU RADIO COMPANION application. From the app there is two empty flow graph then gave a id and title for the option flow graph. The variable properies’s value 32k 
samples per second because its very low so changed it to 2.4mega samples per second.There is no RTL-SDR source then I downloaded it the osmo SDR .Then entered the needed frequency that is available 
in our region .Then added a frequency sink that is basically for showing the spectrum . From the spectrum we can identify the maximum and min frequencies .Then add a GUI RANGE change the values of
default value (FM available frequency) ,start,stop (set a range between the fm frequencies), and add the step .Change all the centre frequencies as the id given in the gui range.
Add another flow graph which is Low pass filter which is used to filter the wanted frequencies and the negative frequencies and high frequencies and connect this with the another frequency sink.
From the graph we can see a hump in the graph then we have to change the hump frequency into the audio signals. Then in the low pass filter change the decimation from 1 to 5 ie: keeping after filtering
only keep 1 in samples that we changed to 5.  After changing that in the graph the hump in the graph is zoomed. Then add a GUI TIME SINK and connected to the low pass filter .  Then added a wide band fm
receive which convert the frequency signal into the audio .Then add the frequency sink and connected to that WBFM receive.Then add a audio sink and connected to the WBFM receive and select the sample 
rate as 48khz which is highest quality audio.Then we run the code we can hear the FM at that frequency .
