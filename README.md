# TextEditor
## Introduction
I’ve always enjoyed building beautiful Graphical User Interfaces (GUIs) to the back-end computations, number-crunching and algorithms of my programs.
This project will be about how to build a simple but useful rich-text editor using Qt Disigner . The first part of the tutorial will focus on the core features and skeleton of the editor. In part two, we’ll take care of text-formatting

 This is what it’ll look like at the end of this project
 
 ![image](https://user-images.githubusercontent.com/75392302/146639061-067010fd-e8f4-4a76-a4ec-00b37d0dc352.png)
 
 Before we get started,  we need to add resources files for icons.
 
 ## Resource Files
The text editor uses several icons to represent various actions. The icons are in the resources directory which is directly under the TextEditor project directory. The images as well as the project files are also listed in the icons documentation .

To register the image files, in the Edit mode, right-click the icons.qrc file and select Open in Editor.

Click the Add button and select Add Files.

In the file manager, select the files to be added.

## Part 1:

We will mainly use the designer for a rapid design of  text editor features.

Set of menus for our application.

![image](https://user-images.githubusercontent.com/75392302/146640973-16b50665-4eb6-4394-82f2-2ad7b563ae36.png)

#### Actions:
now we shoud add actions to every menu

1- file menu's actions:

![image](https://user-images.githubusercontent.com/75392302/146641459-08f7aab2-2c89-45ab-8105-3a6cf224fe89.png)

New : Action to create a new file,

Open : Action to open a  file,

Save & save as :  actions to Save the current file,

Exit : Operation close.

2-Edit menu's actions:

![image](https://user-images.githubusercontent.com/75392302/146642258-6bac8485-cd73-4cc0-b398-3374b0136d01.png)

Cut : Action to cut the text,

Copy : Action to copy the text,

Paste : Action to paste the text,

Select All : Action to select the full text.

3-Text menu's actions:

this menu is about some text's formats like Italic format or underline the text or the color of our text( red , blue , yellow ).

![image](https://user-images.githubusercontent.com/75392302/146642322-38bc5fbf-f3b0-47de-a050-75395eba0dc3.png)

4-View menu's actions:

![image](https://user-images.githubusercontent.com/75392302/146642376-2246f029-a45a-4870-892d-00ca8951a0fd.png)

Undo : Erases the last change made to the text,

Redo : Restores the most recent UNDO operation performed on the text,

Zoom in/out  : for changing font size.


5-Help menu's actions:

in this menu we have just about Action to give some information about the application.

![image](https://user-images.githubusercontent.com/75392302/146642406-1c8ec0b7-6b56-4b39-a5ea-8129eedb376c.png)

## Part 2 :
This part is about TextEditor Class Implementation.

1- Costractor:

In the constructor, we start by creating the actual instances of widgets using setupUi() method **( setupUi() is created for you automatically by UIC (UI compiler - a Qt tool) so we don't have to do that manually. All the properties that we set in QtDesigner and all elements we put there will be "translated" in C++ code )** . Then we call setCentralWidget() to tell that this is going to be the widget that occupies the central area of the main window, between the toolbars and the status bar.



 
