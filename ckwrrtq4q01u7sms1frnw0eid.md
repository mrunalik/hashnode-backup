## Display the copied selected text from notepad in message box.

Today I will write a blog on UiPath Studio. We will see an example of Desktop recording and copy selected text activity. What we are going to do is recording all the activities that we will perform from opening a notepad to copying a text and once the text is copied immediately at that moment display the text that is copied in the message box activity. Then let's start.

1) Open the UiPath Studio and click on process in the right hand side panel. You can see in the snapshot below.

![Screenshot 2021-08-19 123246.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1629356644345/121jrYDku.jpeg)

As soon as you click on the process an message window will pop up asking for process name the location where you want to save the process.

![PROCESS SAVE.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1629356986254/QmqxErIXy.jpeg)

In my case I have given the process name as Example10 you can as you want and also description which is optional. And hit create.

You may see something like this.

![inside.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1629357805983/kjVK4Leahz.png)

Click on new and select flowchart.

![flowchart.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1629358015345/Bb_1CPYoz.jpeg)

A window will pop up asking you to enter the flowchart name and create

![flowchart.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1630323709598/kQiUVOHi_.jpeg)

Similarly Drag and drop sequence activity inside the flowchart activity and set the sequence as start node. (Right Click on sequence and at bottom click on set as start node)

Open the sequence by double clicking on the sequence. Now start desktop recording from the recording option, as shown in below screenshot.

![desktop recording.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1630324180826/S3veEohks.jpeg)

As soon as you click on desktop recording a window will popup as below screenshot.

![click on record.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1630324299542/9LXLscx1l.jpeg)

Click on record .

Now the process start to capture the text.

1) Click on notepad from the taskbar of your PC.
2) Again an window will popup asking to indicate the anchor. Click on "No".

![CLICK ON NO WHEN CLICKED ON NP.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1630324458869/dTtDj6mBf.jpeg)

3) Once the notepad is opened click on the writing area of the notepad. After clicking on the writing area you may see something like below snapshot.


![WRITE TEXT.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1630324599542/u3MKhRlle.jpeg)

Write some text inside the input field and hit enter. In my case I have just typed hello.

4) Click on edit and then click on select all option from the dropdown. Again it will ask for indicate anchor just ignore the message and click again on select all option. And after all the text is selected click "NO" on indicate anchor popup window.
![select all.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1630324847512/N2QBoYcII.jpeg)

![CLICK ON NO WHEN CLICKED ON NP.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1630324938652/DAFJ9b0cT.jpeg)

5) Again Click on edit and then click on "copy" option from the dropdown. Again anchor window will appear but this time click on "indicate anchor".

![click on copy and indecate anchor.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1630326760637/8qOM459ea.jpeg)

6) Thats all in recording part. Press Esc from your keybord until and window popups click on save and exit.

7) In the UiPath studio scroll down at the botton in the desktop recording that we just created. And inside the do part darp and copy selected text activity from the activities panel.

![drag and drop copy selected text.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1630326949640/ui11v-Q9d.jpeg)

Select the copy selected text activity and go to properties panel and set the variable in the result property. I have given the variable name as text. That holds the text that is copied from the notepad.

![create variable that holds the text from notepad.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1630327025433/voC1jf8Nul.jpeg)

8) Drag and drop the message box activity from the activity panel below the copy selected text activity and specify the variable that holds the text from the notepad. For me it is text.

![assign the variable to the message box.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1630327163175/tt41ZFg1y.jpeg)

9) Save and run the project. Make sure before you start debugging your project the notepad should be closed. No file should be opened in the notepad else it shows an error and terminate the execution of the sequence.
If everything goes correctly you will message box at the end of the execution that shows the text that you have written in the notepad.


![output.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1630327361678/dsh945l-c.jpeg)
