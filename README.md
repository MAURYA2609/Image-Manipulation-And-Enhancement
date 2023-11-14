<h1> Image Manipulation And Enhancement </h1>
<p> This is a command line interface (CLI) and Graphical User Interface (GUI) based application, which can be used to load an image from a local system. Perform various basic and advance operations on it such as brightening or darkening of an image, flipping it horizontally or vertically, splitting the red-green-blue channel from an image. Moreover, splitting based on value-component, luma-component and intensity-component as well. It also includes combining the images to get a final combined image. Furthermore, additional advance functionalities include, blurring an image, sharpening an image, dithering it and also converting it to sepia toned image. After performing these operations, one can save the edited image as well. Apart from writing the sequence of such commands, user can directly load the text file of the script and run on our application. To add the visuals to our application, we have designed the UI part using Java Swing.</p>

<h1> Architecture </h1>
<p> This application is designed based on Model-View-Controller (MVC) architecture. But as view require some additional requirements for displaying the data, so putting that code in our model will make it less reusable for other applications. So we have created a ViewModel. The model layer contains the core logic of the application. User interacts with the controller, gives all the inputs to it and then our handler passes that information from controller to model, and model processes it. Furthermore,the view layer handles user interactions and outputs, and the controller layer mediates between the two. Furthere details about ViewModel is described in the Model section. </p>

<h1>Main</h1>
<p> This is the starting point of the Java program and will be executed when the program is ran. In this code, an instance of the AdvImageModelImpl class is created and assigned to a variable named model. This instance is used later to perform operations on images. 
An instance of the ExtensibleImageController class is also created and assigned to a variable named controller. The controller is initialized with either a FileInputStream or the standard input stream (System.in) based on the command line arguments passed to the program.
If the first command line argument is -file, then the second argument is assumed to be the path to the input file. A FileInputStream is created using this path and passed to the controller as the input stream. If any error occurs during this process, an error message is printed and the program exits with a status of 0.
If the first argument is -text, it creates an instance of ExtensibleImageController with System.in (standard input) and a new StringBuffer object.
If there are no command line arguments or the first argument is not -file, then the standard input stream (System.in) is used as the input stream for the controller.
Finally, the imageControllerHandler method of the controller object is called, passing in the model object. </p>

<h1> Controller </h1>
<p> On controller side we have used command design pattern to implement our logic because, the command design pattern has a unifying effect, making unrelated lines of code appear as if working towards the same purpose. This increases cohesion. The command design pattern promotes delegation. Details of each command are now kept in separate classes, instead of all appearing within the controller. Under the package titled controller, we have an interface titled "ImageController" which has a method imageControllerHandler which is implemented by a class named ExtensibleImageController. We also have one another package called commands under controller. Basically, ExtensibleImageController class overrides a handler which manages a map which contains key as a name of command and a function as a value which contains the arguments of that command and accordingly calls the command. Under the commands package, we have all the classes of possible functionalities which our application provides. All the class has a public constructor which is used to initialize the given arguments and one another argument called "executeCommand" which calls the model. There is this interface called ImageOperationCommand which has this method executeCommand and all our classes under command package implements that interface. Apart from load and save, all other commands call the model directly. For load and save we are calling ImageRead and ImageWrite respectively from utils package. To incorporate the view in our application, 
we have created a new controller titled "ViewControllerImpl" which implements our new interface "ViewController". This interface represents the controller for the view. View calls these methods, and it will perform the tasks such as loading an image, setting a view, performing all the basic and advance operations which have already implemented.
Apart from these methods, it has a method called setCurrentImageName which sets the current image being showed to the user. The ViewControllerImpl class performs all the actions performed by user via GUI. In short, our new controller calls the previous one by passing the required arguments using StringReader when performTask method is called. </p>

<h1>Model</h1>
<p> The model layer is implemented using functional programming methodology which contains an interface titled ImageModel, basically it contains the implementation part of all the basic functionalities such as loading an image, brighten, flipping horizontally, vertically, splitting into RGB channels, combining the images, saving it and also running it by loading a script file from local machine in a class called ImageModelImple. Furthermore, there is this interface called AdvImageModel which extends ImageModel. AdvImageModel has all the advanced functionalities such as blurring an image, sharpening it, dithering it and applying sepia tone to it. All these advanced functionalities have been implemented in the class AdvImageModelImpl. So, AdvImageModelImpl extends our ImageModelImpl class and implements the AdvImageModel interface. So when we create an object of AdvImageModelImp, we can access all the basic as well as advacned functionlaities. By using functional programming, we have created a modular and extensible image processing system that is easier to understand, maintain, and test. To amalgamate our view in the previous design, we have created a new read only model titled "ViewModelImpl" which implements "ViewModel" interface. This interface has only one method which is used to read and it returns the current map of image data. </p>

<h1>utils</h1>
<p> This package basically stores all the details of an image under a class called ImageDetails which implements an interface called "Image". In this class, we store the format of an image, it height, width, maxValue of pixels and a 3D array of pixels. Furthermore, this package also contains an interface called ImageHandler, it is used to get all the image details in one go. So it contains only one method titled getImageDetails. There are two classes titled "ImageRead" and "ImageWrite" which implements this ImageHandler interface. These are used to load an image and save an image. Based on the extension of an image, ImageRead calls the respective helper method. And same goes with the ImageWrite as well. To add one more functionality of displaying a Histogram, we have created a class called Histogram which extends JPanel and takes an array of long as input which is hostograms data. Furthermore, it calls the paint method which draws axes, lines and labels for each of the four histograms using Graphic2D class.</p>

<h1> View </h1>
<p> Under view package, we have our logic of building UI for our application. We have an interface which can be implemented by class that needs to display images in GUI and handle the interactions with those image. addFeautures can be used to add a controller to the view that can respond to user interactions. showImageData is used to display the image data and showError is used to display the error. Class titled "ImageViewImpl" extends JFrame and implements ImageView. This class is basically a class which has all the implementation part of our GUI. It has panels, buttons, labels, texts, dialogs and all logic of the view. Furthermore, it has all the addActionListeners for the buttons which perform the desired tasks. Along with that it has showImageData which display the image and the histogram. And shows error when it occurs.</p>

<h1> Additions/Changes and Justifications </h1>
<p>1. Added a new controller for view which has methods that works with operations that are performed via GUI.</p>
<p>2. Added a new read-only model to keep the view's required code for displaying data and keeping the model's code reusable. To summarize, view won't edit the models code, instead will read from our read-only model.</p>
<p>3. Added a class Histogram under utils which generates a histogram.</p>
<p>4. Created a new package view and added ImageViewImpl class which implements all the graphical user functionalities for the end user. Furthermore, it implements the interface ImageView which can be used to add controller to the view, show error messages and display image data.</p>
<p>5. Updated our main method to support backward compatibility in our application and run it using CLI as well as GUI using first argument of .jar file of application.</p>

<h1> Overview of current Design </h1>
<p> Now as we have our most refined design, adding a new functionlity is going to be pretty simple as we are still adhereing to the MVC architecture. One need to just add the command line signature to our map in ExtensibleImageController class and create a respective class for that in commands package. And to add multiple new functionlities on top of these old ones, one can create a new interface and implement that in a new class which extends the old ones. So doing these doesn't change neither changes our previous code nor our design. Furthermore, to add our support for other formats of images, we just need to add that in ImageRead and ImageWrite class, thats's it! Nothing to change anywhere else. Now as we have seperate controller for our view, and a seperate read-only model for it,  its quite starightforward to add new functionality in our GUI part as well and display its histogram. </p>

<h1>Image Citation</h1>
<p>The image which we have used, was captured by my friend, and features myself. I and Maurya give permission to use this photo.</p>
                                       
                                     
