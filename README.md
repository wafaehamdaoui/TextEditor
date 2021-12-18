# TextEditor
## Introduction
I’ve always enjoyed building beautiful Graphical User Interfaces (GUIs) to the back-end computations, number-crunching and algorithms of my programs.
This project will be about how to build a simple but useful rich-text editor using Qt Disigner . The first part of the project will focus on the core features and skeleton of the editor. In part two, we’ll take care of text-formatting

 This is what it’ll look like at the end of this project
 
 ![image](https://user-images.githubusercontent.com/75392302/146639061-067010fd-e8f4-4a76-a4ec-00b37d0dc352.png)
 
 Before we get started,  we need to add resources files for icons.
 
 ## Resource Files
The text editor uses several icons to represent various actions. The icons are in the resources directory which is directly under the TextEditor project directory. 

To register the image files, in the Edit mode, right-click the icons.qrc file and select Open in Editor.

Click the Add button and select Add Files.

In the file manager, select the files to be added.

## Part 1:

We will mainly use the designer for a rapid design of  text editor features.

Set of menus for our application.

![image](https://user-images.githubusercontent.com/75392302/146640973-16b50665-4eb6-4394-82f2-2ad7b563ae36.png)

#### Actions:
now we shoud add actions to every menu

***1- file menu's actions:***

![image](https://user-images.githubusercontent.com/75392302/146641459-08f7aab2-2c89-45ab-8105-3a6cf224fe89.png)

New : Action to create a new file,

Open : Action to open a  file,

Save & save as :  actions to Save the current file,

Exit : Operation close.

***2-Edit menu's actions:***

![image](https://user-images.githubusercontent.com/75392302/146642258-6bac8485-cd73-4cc0-b398-3374b0136d01.png)

Cut : Action to cut the text,

Copy : Action to copy the text,

Paste : Action to paste the text,

Select All : Action to select the full text.

***3-Text menu's actions:***

this menu is about some text's formats like Italic format or underline the text or the color of our text( red , blue , yellow ).

![image](https://user-images.githubusercontent.com/75392302/146642322-38bc5fbf-f3b0-47de-a050-75395eba0dc3.png)

***4-View menu's actions:***

![image](https://user-images.githubusercontent.com/75392302/146642376-2246f029-a45a-4870-892d-00ca8951a0fd.png)

Undo : Erases the last change made to the text,

Redo : Restores the most recent UNDO operation performed on the text,

Zoom in/out  : for changing font size.


***5-Help menu's actions:***

in this menu we have just about Action to give some information about the application.

![image](https://user-images.githubusercontent.com/75392302/146642406-1c8ec0b7-6b56-4b39-a5ea-8129eedb376c.png)

### Toolbar 

For our application, we will add main toolbar with the following actions:

![image](https://user-images.githubusercontent.com/75392302/146644153-b1a1bb98-b408-479c-8dee-4c15c7e0d9fc.png)



## Part 2 :
This part is about TextEditor Class Implementation.

### Constractor:

In the constructor, we start by creating the actual instances of widgets using setupUi() method **( setupUi() is created for you automatically by UIC (UI compiler - a Qt tool) so we don't have to do that manually. All the properties that we set in QtDesigner and all elements we put there will be "translated" in C++ code )** . Then we call setCentralWidget() to tell that this is going to be the widget that occupies the central area of the main window, between the toolbars and the status bar.

```cpp
TextEditor::TextEditor(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::TextEditor)
{
    ui->setupUi(this);
    setCentralWidget(ui->textEdit);
}
```

### New File:

```cpp
void TextEditor::on_actionNew_triggered()
{
    currentFile.clear();
    ui->textEdit->setText(QString());
}

```
The newFile slot is invoked when the user selects File|New from the menu.
we clear the QPlainTextEdit 

![image](https://user-images.githubusercontent.com/75392302/146650948-b752f564-45f8-4cc1-8421-4f0643751500.png)![image](https://user-images.githubusercontent.com/75392302/146650977-8ca3da28-ee7f-4dd1-a012-6bc248a3e587.png)



### Open File:
```cpp
void TextEditor::on_actionOpen_triggered()
{
    QString filename = QFileDialog::getOpenFileName(this,"open the file");
    QFile file(filename);
    currentFile = filename;
    // Try to open the file as a read only file if possible or display a
        // warning dialog box
        if (!file.open(QIODevice::ReadOnly | QFile::Text)) {
            QMessageBox::warning(this, "Warning", "Cannot open file: " + file.errorString());
            return;
        }

        // Set the title for the window to the file name
        setWindowTitle(filename);

        // Interface for reading text
        QTextStream in(&file);

        //  Copy text in the string
        QString text = in.readAll();

        // Put the text in the textEdit widget
        ui->textEdit->setText(text);

        // Close the file
        file.close();

}

```
In on_actionOpen_triggered(), we use QFile and QTextStream to read in the data. The QFile object provides access to the bytes stored in a file,

 We pop up a QFileDialog asking the user to choose a file. If the user chooses a file ,

We start by Trying to open the file as a read-only mode file if possible or display a warning dialog box,

If we successfully opened the file, we use a QTextStream object to read in the data. **QTextStream automatically converts the 8-bit data into a Unicode QString and supports various encodings. If no encoding is specified, QTextStream assumes the file is written using the system's default 8-bit encoding ( see https://doc.qt.io/qt-5/qtextcodec.html for more details)**,

Then we Put the text in the textEdit widget using setText() method,

The open slot is invoked when the user clicks File|Open. And then we can choose the file that we want to resad.

![image](https://user-images.githubusercontent.com/75392302/146651024-045b5261-1a7a-4fb0-b9ea-b3684900377d.png)

![image](https://user-images.githubusercontent.com/75392302/146651081-c2e35c77-aa2b-4b4c-a1a5-d91fb6dfd458.png)

### Save 
```cpp
void TextEditor::on_actionSave_triggered()
{
    QString fileName = QFileDialog::getSaveFileName(this, "Save");
        QFile file(fileName);
        if (!file.open(QFile::WriteOnly | QFile::Text)) {
            QMessageBox::warning(this, "Warning", "Cannot save file: " + file.errorString());
            return;
        }
        currentFile = fileName;
        setWindowTitle(fileName);
        QDataStream out(&file);
        QString text = ui->textEdit->toPlainText();
        out << text;
        file.close();

}

```

In on_actionSave_triggered(), we use QFile and QDataStream to read in the data. 

We start by Trying to write the file as a write-only mode file if possible or display a warning dialog box.

If we successfully write the file, we use a QDataStream object to write on the data.

The save slot is invoked when the user clicks File|Save. Then we name our file and save it. 

![image](https://user-images.githubusercontent.com/75392302/146651024-045b5261-1a7a-4fb0-b9ea-b3684900377d.png)

![image](https://user-images.githubusercontent.com/75392302/146653416-91302b6c-7692-411f-9973-5d61d0c363be.png)


### Exit:

```cpp
void TextEditor::on_actionExit_triggered()
{
    QApplication::quit();
}
```
**When the user attempts to close the window, we call the private slot quit().**

### Cut :
```cpp
void TextEditor::on_actionCut_triggered()
{
    ui->textEdit->cut();
    //ui->textEdit->copy();
    //ui->textEdit->delete();
    
}

```
**When the user wants to cut the text, we call the private slot cut(). ( we can also execute copy() and delete() slots.)**

First we select the part that we want to cut, then we click Edit|Cut 

![image](https://user-images.githubusercontent.com/75392302/146651651-c219d674-5b99-40ef-80e3-47a52e992286.png)

![image](https://user-images.githubusercontent.com/75392302/146651716-97481272-29c9-4a0f-ac3a-cc66efbdc9cb.png)

![image](https://user-images.githubusercontent.com/75392302/146651734-d31aecea-2902-4ebf-a520-88a430e48705.png)

### copy
```cpp
void TextEditor::on_actionCopy_triggered()
{
    ui->textEdit->copy();
}

```
**When the user wants to copy the text, we call the private slot copy().**

### Paste
```cpp
void TextEditor::on_actionCopy_triggered()
{
    ui->textEdit->paste();
}

```
**When the user wants to paste a text that is coped or cuted , we call the private slot paste().**

###  Select All
```cpp
void TextEditor::on_actionSelect_All_triggered()
{
  ui->textEdit->selectAll();
}
```
**When the user wants to select the full text , we call the private slot selectAll().**

First we select the full text:

![image](https://user-images.githubusercontent.com/75392302/146651803-179e18d4-68de-4815-984f-b27259a79cd8.png)

Then we click Edit|Copy:

![image](https://user-images.githubusercontent.com/75392302/146651877-f0668606-e65e-4ea3-872e-76bdbfe82c1b.png)

And we click Edit|Paste:

![image](https://user-images.githubusercontent.com/75392302/146651924-d16c9f0d-d51c-46e9-a2cd-cb1798d7a571.png)

Here is the result:

![image](https://user-images.githubusercontent.com/75392302/146651976-a51f070e-4a6a-417a-bef0-abf9fad23d28.png)



### Zoom in
```cpp
void TextEditor::on_actionZoom_in_triggered()
{
    ui->textEdit->zoomIn();
}

```
**When the user wants to increase the size of text the full text , we call the private slot zoomIn().**

![image](https://user-images.githubusercontent.com/75392302/146652220-9d3843ba-5e96-4e19-a150-7ecf5489db97.png)

![image](https://user-images.githubusercontent.com/75392302/146652226-0218953e-d4b9-45b7-8b85-b4e986b8cc7e.png)


### Zoom out
```cpp
void TextEditor::on_actionZoom_out_triggered()
{
    ui->textEdit->zoomout();
}

```
**When the user wants to decrease the size of text , we call the private slot zoomOut().**

![image](https://user-images.githubusercontent.com/75392302/146652217-b6c4d3b2-74f6-450a-b9db-990509eb8c95.png)


![image](https://user-images.githubusercontent.com/75392302/146652201-e5a93252-58cf-415b-87c8-d729bf34d7f8.png)


### Italic font
```cpp
void TextEditor::on_actionItalic_triggered()
{
    ui->textEdit->setFontItalic(true);
}

```
**When the user wants to change the font of text to italic format , we call the private slot setFontItalic() and give it "true" as a parameter.**

First we write  a text:

![image](https://user-images.githubusercontent.com/75392302/146652265-f0661eef-5f61-4695-b962-fb35c95a83e0.png)

Then I select the words that I want change their format to iltalic format 

![image](https://user-images.githubusercontent.com/75392302/146652361-56b417cf-8916-4c07-bb63-554c605144c7.png)

Finally we click on Text|Italic:

![image](https://user-images.githubusercontent.com/75392302/146652445-f720235f-f7e3-45cf-a17e-f7b6c966794b.png)


### underline font
```cpp
void TextEditor::on_actionunderline_triggered()
{
    ui->textEdit->setFontUnderline(true);
}

```
**When the user wants to change the font of text (underline the text) , we call the private slot setFontUnderline() and give it "true" as a parameter.**

First we write  a text:

![image](https://user-images.githubusercontent.com/75392302/146652265-f0661eef-5f61-4695-b962-fb35c95a83e0.png)

Then I select the words that I want to underline 

![image](https://user-images.githubusercontent.com/75392302/146652502-63ea6c0a-7841-4e30-a72f-502c5ab36ea8.png)

Finally we click on Text|Underline:

![image](https://user-images.githubusercontent.com/75392302/146652525-b9d589a3-88fd-4899-8561-83b265953bc6.png)


### Color's text
For color action we add tree color **red, blue, yellow,** and we can select the color that we want to write with.

**red**
```cpp
void TextEditor::on_actionRed_triggered()
{
    ui->textEdit->setTextColor(QColor(255,0,0));
}

```
**When the user wants to change the color of text to red , we call the private slot setTextColor() and give it a QColor(The QColor class provides colors based on RGB, HSV or CMYK values) as a parameter .**
 
 First we write a text,and click on select all, then we go to text menu and color action , and select red one 
 
 ![image](https://user-images.githubusercontent.com/75392302/146647655-9c4a6949-521b-461c-ba71-6b7c9f972767.png)
 
 ![image](https://user-images.githubusercontent.com/75392302/146647777-3206ebbd-d757-4223-bd2f-517359b60863.png)
 
 ![image](https://user-images.githubusercontent.com/75392302/146647721-b3e71d41-8d67-4875-a943-93fab0d4fe62.png)
 
 ![image](https://user-images.githubusercontent.com/75392302/146647821-d6f35df7-ee61-40d7-9a09-c2d0fcf8841b.png)

**yellow**
```cpp
void TextEditor::on_actionyellow_triggered()
{
    ui->textEdit->setTextColor(QColor(255,255,0));
}

```
**When the user wants to change the color of text to yellow , we call the private slot setTextColor() and give it a QColor(The QColor class provides colors based on RGB, HSV or CMYK values) as a parameter .**
 
 First we write a text,and click on select all, then we go to text menu and color action , and select yellow one 
 
 ![image](https://user-images.githubusercontent.com/75392302/146649392-3e6aaa68-d8d0-4605-8351-bbc942411a33.png)

![image](https://user-images.githubusercontent.com/75392302/146649413-e751ece8-673b-4d33-9df9-bab344c2a888.png)

![image](https://user-images.githubusercontent.com/75392302/146649483-fc769862-75c7-483f-98a4-b1d537693c4b.png)

![image](https://user-images.githubusercontent.com/75392302/146649508-42cebb55-ec0b-4552-9076-69eeba517e60.png)

**Blue**
```cpp
void TextEditor::on_actionblue_triggered()
{
    ui->textEdit->setTextColor(QColor(0,0,255));
}

```
**When the user wants to change the color of text to yellow , we call the private slot setTextColor() and give it a QColor(The QColor class provides colors based on RGB, HSV or CMYK values) as a parameter .**

 First we write a text,and click on select all, then we go to text menu and color action , and select blue one
 
 ![image](https://user-images.githubusercontent.com/75392302/146649621-ffc67855-2db4-45be-a2e5-180b1863c8cb.png)

![image](https://user-images.githubusercontent.com/75392302/146649631-b1d178c0-9b74-423f-b319-e6a7b8349ac7.png)

![image](https://user-images.githubusercontent.com/75392302/146649686-8c4afac2-acae-4b07-bd7e-3dbff18cb7c8.png)

![image](https://user-images.githubusercontent.com/75392302/146649713-df47808c-1385-4a62-89b4-61ca33e94638.png)

**Undo**
```cpp
void TextEditor::on_actionundo_triggered()
{
    ui->textEdit->undo();
}

```
**When the user wants to Erase the last change made on the text , we call the private slot undo().**

**Redo**
```cpp
void TextEditor::on_actionredo_triggered()
{
    ui->textEdit->redo();
}

```
**When the user wants to cancel recent UNDO operation performed on the text , we call the private slot redo().**

### About Action
```cpp
void TextEditor::on_actionAbout_triggered()
{
    QMessageBox::about(this, "about me","I'm trying to create a TextEditor applicaton ");
}

```
**The application's About box is done using one statement, using the QMessageBox::about() function .**

![image](https://user-images.githubusercontent.com/75392302/146646941-f8f34adc-d460-43f0-b3f3-4fd5caae9673.png)



**Thank you**

> Amazing dedication as usual. Keep up the good work and hope it's not all for the project as we still have some missing functinalities in the Spreadsheet application. Again Excellent work!
