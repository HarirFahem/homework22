# Signals And Slots


- [Introduction](#INTRO)
- [Traffic Light](#TrLight)
- [Digital Clock](#DigiClock)
- [Calculator](#Calcul)

<div id = "back"></div>


    
## **Introduction** 



<a name="INTRO"></a>
    
"" **All programming is maintenance programming, because you are rarely writing original code.**""

A **widget** is an element of a graphical user interface (**GUI**) which displays information or provides a specific way for a user to interact with the operating system or an application. 

**Widgets** include icons, pull-down menus, buttons, selection boxes, progress indicators, on-off checkmarks, scroll bars, windows, window edges, toggle buttons, form, and many other devices for displaying information and for inviting, accepting, and responding to user actions.

In **programming**, widget also means the small program that is written in order to describe what a particular widget looks like, how it behaves and how it responds to user actions. Most operating systems include a set of ready-to-tailor widgets that a programmer can incorporate in an application, specifying how it is to behave. New widgets can be created.

The **Layout Widget** is a responsive container that allows you to separate a responsive container into sections. Layouts can be placed inside the responsive containers of other layouts to create more even sections.

The Header and Footer have constant heights with responsive widths, while the sidebars have a constant width and responsive heights. The Columns and rows have both responsive widths and heights, but you can change the percent of available space they will occupy relative to the space not being occupied by headers, footers, and sidebars.


![Image](/intro.png)

Example of a Form 
    

 
 >**In This Zip you will have the project** [WidgetLayout.zip]() 

### Traffic Light
<a name="TraLight"></a>

**Traffic Light** is a set of automatically operated coloured lights, typically red, yellow, and green, for controlling traffic at road junctions, pedestrian crossings, and roundabouts.

In this Exercise, we used the QTimer to simulate the traffc light.

We created a Class called **traffic light**.
Here is the code for the class:

```javascript
class TrafficLight: public QWidget{
  Q_OBJECT

public:

  TrafficLight(QWidget * parent = nullptr);
  void timerEvent(QTimerEvent *e) override;
  void keyPressEvent(QKeyEvent *e) override;

protected:
     void createWidgets();
     void placeWidgets();
     void makeConnexions();

private:

  QRadioButton * redlight;
  QRadioButton * yellowlight;
  QRadioButton * greenlight;
  //QVector<QRadioButton*> lights;
  int times[3]={4,1,2}; //temps de chacune
  int index;//indice du light active
  int currentTime;//temps d'activation du feu

};

```
And here is the implementation of the methods:
```javascript

TrafficLight::TrafficLight(QWidget * parent): QWidget(parent){

    //Creatign the widgets
    createWidgets();

    //place Widgets
    placeWidgets();
  // startTimer(1000);
   index=0;
   //currentTime=0;
}

void TrafficLight::createWidgets()
{

  redlight = new QRadioButton;
  redlight->setEnabled(false);
  redlight->toggle();//activer le radio Button
  redlight->setStyleSheet("QRadioButton::indicator:checked { background-color: red;}");

  yellowlight = new QRadioButton;
  yellowlight->setEnabled(false);
  yellowlight->toggle();
  yellowlight->setStyleSheet("QRadioButton::indicator:checked { background-color: yellow;}");

  greenlight = new QRadioButton;
  greenlight->setEnabled(false);
  greenlight->toggle();
  greenlight->setStyleSheet("QRadioButton::indicator:checked { background-color: green;}");
  startTimer(1000);
  currentTime=0;
 /* //ajouter les lights dans un tableu
  lights.append(redlight);
  lights.append(yellowlight);
  lights.append(greenlight);
  */

}


void TrafficLight::placeWidgets()
{

  // Placing the widgets
  auto layout = new QVBoxLayout;
  layout->addWidget(redlight);
  layout->addWidget(yellowlight);
  layout->addWidget(greenlight);
  setLayout(layout);
}
void TrafficLight::keyPressEvent(QKeyEvent *e){


//traiter le cas du rouge

   /* if(e->key() == Qt::Key_R){
        index=0;
        lights[index]->toggle();
    }
    if(e->key() == Qt::Key_Y){
        index=1;
        lights[index]->toggle();
    }
    if(e->key() == Qt::Key_G){
        index=2;
        lights[index]->toggle();
    }
*/

    if(e->key() == Qt::Key_Escape)
        qApp->exit();
}

void TrafficLight::timerEvent(QTimerEvent *e){
    //index light active
    // index=(index+1)%3;
    //lights(vecteur des lights)
     //lights[index]->toggle();
    currentTime++;
     if(redlight->isChecked()&&currentTime==4){
         yellowlight->toggle();
         currentTime=0;
     }
     if(yellowlight->isChecked()&&currentTime==1){
         greenlight->toggle();
         currentTime=0;
     }
     if(greenlight->isChecked()&&currentTime==2){
        redlight->toggle();
         currentTime=0;
     }

}

```
And for showing the window we wrote in the main class the following code:
```javascript

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    //Creating the traffic light
    auto light = new TrafficLight;
    //showing the trafic light
    light->show();
    return a.exec();
}
```
And here is the Form that we Obtain :

![Image](/traffic_light.png)

A QHBoxLayout example
    

 [(**Back to top**)](#back)

## Nested Layouts 
<a name="DigiClock"></a>
    
First we analyzed this following construction of a form:

![Image](/nestedlayout.png)

Second we coded it using **Nested Layout**:
For adding a **layout** to a main one , we used: 
```markdown
addLayout(layout);
```
For the **vertical space** the strech , we added it by:
```markdown
layout->addSpacer(SpaceItem)
```
Here is the code of this class:
```javascript
class Dialog1 : public QWidget
{

public:
    explicit Dialog1(QWidget *parent = nullptr);
virtual ~Dialog1();
protected:
  void createWidgets();
  void placeWidgets();
  void makeConnexions();

protected:
  QLabel *label;
  QLineEdit *line;
  QPushButton *search;
  QPushButton *close;
  QCheckBox *match;
  QCheckBox *backword;
  QHBoxLayout *layoutP;
  QVBoxLayout *layoutS;
};
```
Then , here is the implementation of the methods:
```javascript
Dialog1::Dialog1(QWidget *parent) : QWidget(parent)
{
    createWidgets();
    placeWidgets();
    makeConnexions();
}

Dialog1::~Dialog1(){
delete search;
    delete close;
    delete label;
    delete match;
    delete backword;
    delete layoutP;
    delete layoutS;
    delete line;
}
void Dialog1::createWidgets(){
    label = new QLabel("Name:");
    line = new QLineEdit();
    search = new QPushButton("Search");
    close = new QPushButton("Close");
    match = new QCheckBox("Match case");
    backword = new QCheckBox("Search Backward");

   layoutP = new QHBoxLayout();
   layoutS = new QVBoxLayout();
    }
void Dialog1::placeWidgets(){
    auto mainLayout = new QHBoxLayout;
    auto leftLayout = new QVBoxLayout;
    auto topLeftLayout = new QHBoxLayout;
    auto RightLayout = new QVBoxLayout;

          setLayout(mainLayout);
          leftLayout->addLayout(topLeftLayout);
          leftLayout->addWidget(match);
          leftLayout->addWidget(backword);

          topLeftLayout->addWidget(label);
          topLeftLayout->addWidget(line);

          RightLayout->addWidget(search);
          RightLayout->addWidget(close);
          RightLayout->addSpacerItem(new QSpacerItem(10,10,QSizePolicy::Fixed,QSizePolicy::Expanding));


          mainLayout->addLayout(leftLayout);
          mainLayout->addLayout(RightLayout);
}

void Dialog1::makeConnexions(){

}
```
And for the main part:
```javascript
int main(int argc, char *argv[])
{
    QApplication app(argc, argv);
    auto  D = new Dialog1;
       D->setWindowTitle("Netsted layout test");
       D->show();
    return app.exec();
}
```
Finally, Here is the Form that we get:

![Image](/nestedlayouttest.png)

Nested Layout
    


[(**Back to top**)](#back)

##  Bug Report Form 

<a name="Calcul"></a>
    
This example is taken from [Qt_Tutorial](https://doc.qt.io/archives/qq/qq25-formlayout.html)

We created the following form to report a problem:

![Image](/reportbug.png)

Dialog to report a Form

Here is the code for this class:
```javascript
class Dialog2 : public QWidget
{

public:
    explicit Dialog2(QWidget *parent = nullptr);
protected:
  void createWidgets();
  void placeWidgets();
  void makeConnexions();
protected:
         QLineEdit *name;
         QLineEdit *company;
         QLineEdit *phone;
         QLineEdit *email;
         QLineEdit *problem;
         QTextEdit *summary;
         QComboBox *reproducibility;
         QDialogButtonBox *buttonBox;
};
```
Then , here is the implementation of the methods:
```javascript

Dialog2::Dialog2(QWidget *parent) : QWidget(parent)
{
    createWidgets();
    placeWidgets();
    makeConnexions();

}
void Dialog2::createWidgets(){
            name = new QLineEdit;
            company = new QLineEdit;
            phone = new QLineEdit;
            email = new QLineEdit;
            problem = new QLineEdit;
            summary = new QTextEdit;

            reproducibility = new QComboBox;
            reproducibility->addItem("Always");
            reproducibility->addItem("Sometimes");
            reproducibility->addItem("Rarely");
            buttonBox = new QDialogButtonBox;
            buttonBox->addButton(("Submit Bug Report"),
                                         QDialogButtonBox::RejectRole);
            buttonBox->addButton(("Don't Submit"),
                                         QDialogButtonBox::RejectRole);
            buttonBox->addButton(QDialogButtonBox::Reset);
}
void Dialog2::placeWidgets(){
    QFormLayout *layout = new QFormLayout;
            layout->addRow(("&Name:"), name);
            layout->addRow(("&Company:"), company);
            layout->addRow(("&Phone:"), phone);
            layout->addRow(("&Email:"), email);
            layout->addRow(("Problem &Title:"), problem);
            layout->addRow(("&Summary Information:"),
                           summary);
            layout->addRow(("&Reproducibility:"),
                           reproducibility);

            QVBoxLayout *mainLayout = new QVBoxLayout;
            mainLayout->addLayout(layout);
            mainLayout->addWidget(buttonBox);
            setLayout(mainLayout);



}
void Dialog2::makeConnexions(){

}
```
And in the main class, we wrote those lines to call the methods and showing the result:
```javascript
int main(int argc, char *argv[])
{
    QApplication app(argc, argv);
   auto  D = new Dialog2;
      D->setWindowTitle("Report Bug");
       D->show();
    return app.exec();
}
```
and here is the form that we obtained:

![Image](/reportbugform.png)
    
    
[(**Back to top**)](#back)
 

## Grid Layout 

<a name="GridLayout"></a>
The CSS Grid Layout Module offers a grid-based layout system, with rows and columns, making it easier to design web pages without having to use floats and positioning. A grid layout consists of a parent element, with one or more child elements. An HTML element becomes a grid container when its display property is set to grid or inline-grid.All direct children of the grid container automatically become grid items.

    
In this part, we tried to construct a calculator , the component showing the number is called a **LCD Number** , we called the method **setMinimumHeight** on our LCDNumber to set a minimum height of 80 pixels.

The code of this Class is :

```javascript
class Dialog3 : public QWidget
{

public:
    explicit Dialog3(QWidget *parent = nullptr);

protected:
    void createWidgets();
    void placeWidget();


private:
    QGridLayout *buttonsLayout;
    QVBoxLayout *layout;
    QVector<QPushButton*> digits;
    QPushButton *enter;

    QLCDNumber *disp;
};
```
And here is the implementation of these methods:
```javascript
Dialog3::Dialog3(QWidget *parent) : QWidget(parent)
{
    createWidgets();
    placeWidget();

}

void Dialog3::createWidgets()
{

    layout = new QVBoxLayout();
    layout->setSpacing(2);


    buttonsLayout = new QGridLayout;



    for(int i=0; i < 10; i++)
    {
        digits.push_back(new QPushButton(QString::number(i)));
        digits.back()->setSizePolicy(QSizePolicy::Expanding, QSizePolicy::Fixed);
        digits.back()->resize(sizeHint().width(), sizeHint().height());
    }

    enter = new QPushButton("Enter",this);
    enter->setSizePolicy(QSizePolicy::Expanding, QSizePolicy::Fixed);
    enter->resize(sizeHint().width(), sizeHint().height());


    //creating the lcd
    disp = new QLCDNumber(this);
    disp->setDigitCount(6);
    disp->setMinimumHeight(80);

}
void Dialog3::placeWidget()
{

    layout->addWidget(disp);
    layout->addLayout(buttonsLayout);


    //adding the buttons
    for(int i=1; i <10; i++)
        buttonsLayout->addWidget(digits[i], (i-1)/3, (i-1)%3);

    //Adding the 0 button
    buttonsLayout->addWidget(digits[0], 3, 0);
    buttonsLayout->addWidget(enter, 3, 1, 1,2);

    setLayout(layout);
}
```
And here is the part of the main class:
```javascript
int main(int argc, char *argv[])
{
    QApplication app(argc, argv);
    auto  D = new Dialog3;
       D->setWindowTitle("Numeric Keypad");
       D->show();

    return app.exec();
}
```
    
    
    
After the compilation, we get this form:

![Image](/numerickeypad.png)
  
Calculator using Grid Layout
    


 In this project we analyzed and coded many forms using the widget layout.
    
   
    
 [(**Back to top**)](#back)
    
Made By:
* Khadija Fahem
* Wafa Harir

    
  
