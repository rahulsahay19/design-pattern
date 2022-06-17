```java
class Point2D {
    int x;
    int y;
}

class Shape2D {
    String name;
    Point2D location;

    void caculateArea(){
        //some logic
    }
}

class Sqaure extends Shape2D {
    @Override
    void caculateArea(){
        //This will call Shape2D's implmentation
        super.caculateArea();
    }
}

class Client {
    void foo() {
        Square sq = new Square();
        //this will call the square's implementation
        sq.caculateArea();
    }
}

class Beak{
    Shape shape;
    Color color;
    Hardness hardness;
}

class Wing {
    int featherCount;
    int wingSpan;
    Color color;
}

//classes are concepts with attributes, behaviours, releationships
class Bird {
    //attributes
    String color;
    float weight;
    int age;
    String name;
  
    //releationships modeled in a code via attributes
    //A bird has a beak
    Beak beak;
    Wing leftWing, rightWing;
    Leg leftLeg, rightReg;
    //behaviours
    void fly(){
        print("flap wings");
    }
    void eat(){
        print("peck at the ground");
    }
}

class Sparrow extends Bird {
    //every sparrow object is also a bird object as well
    //whatever a bird can do, a sparrow can also do
    //extra attributes
    String favoriteGrains, favoriteWorm 
    //extra relationships
    Tail tail;
    //extra behaviours
    void chirp(){

    }

}

class Client {
    void foo(){
        Bird b = new Bird();
        b.eat();
        b.name = "My Little Bird";
    }
}

//schema enforcement can be provided by using abstract classes or interfaces.
abstract class Bird {
    abstract void fly(); //no implementation
}

class Sparrow extends Bird {
    void fly(){
        //providing implementation
    }
}

abstract class BlueBird extends Bird {
    //since this class is not providing implmementation of Bird,
    //hence marked as abstract
}

class client{
    void foo(){
        Bird b = new Bird(); // you can't use like this. This is an error
        BlueBird b = new BlueBird(); //This is also an error
        Sparrow s = new Sparrow(); //This is fine
        //This doesn't mean that abstract class can't have objects. abstract classes can't be instantiated but it can certainly have objects
        //Here, I am instantiating an sparrow but creating a Bird object.
        Bird b = new Sparrow(); //This is fine
    }
}

//Some birds can't fly
//in this case schema enforcement is a bad design
class Kiwi extends Bird {
    void fly(){
        throw new CantFlyException("I can't fly, I have no wings");
    }
}

//Basically, it says that you should not enforce your children/clients to implement those methods which they don't want to implement
//There are two ways to avoid this.
//1) via class hierarchy
//But, this kind of approach will create 2^n sub classes as for different attributes, you will be maintaining different sub classes
//This problem is also known as combinatorial explosions
class Bird {

}

//This is fine an abstract simple class can extend concrete class
abstract class FlyingBird extends Bird {
    abstract void fly();
}


class Sparrow extends FlyingBird {
    void fly(){
        print("I can fly");
    }
}

class Kiwi extends Bird {
    //No need to implement fly over here
}

//2) another way of solving this is via interfaces
class Bird(){}
    interface Flight {void fly();}
    interface Swim {void swim();}
    interface Run {void run();}
    interface Peck {void peck();} 
    interface HasWing {void getWings();};

//so you are allowed to extend one class but implement multiple interfaces
class Sparrow extends Bird implements Flight, Peck, HasWings {
    void fly(){}
    void peck(){}
    void getWings(){}
}

//java or C# doesn't allow multiple inheritance as this will lead to diamond problem.
//Polymorphism is divided into two categories compile time and runtime. 
//compile time --> method overloading or operator overloading
//runtime --> virtual/overriding 

class/interface  | Abstraction --> Hiding details
inheritance | Generalization (is - a relationship) --> Reusable code / one thing serves multiple purposes
access Modifiers | Encapsulation --> To wrap with something also data hiding
attributes | Association (has - a relationship) --> Connecting multiple concepts/objects together
           | Aggregation - uses relationship
           | Composition - uses relationship but also owns relationship

Generalization and Composition are sub types of association

class Student {
    List<Course> coursesEnrolledIn; //aggregation -->student uses the course but doesn't own it
}

class Course {
    List<Lecture> lectures; //composition --> a lecture can't exist if course doesn't exist
    List<Student> studentsEnrolled; //aggregation --> course uses the student but doesn't own it
}

class Lecture{
    Course course; //aggregation 
}

//Abstraction:- Hiding the inner implementation detaills. Exposing only what the client is going to use
class Car {
    Tire leftFrontTire;
    Window window1;
    void start();
    void honk();
    //using access modifier to enforce abstract
    private void InternalDetails();
}
//Generalization:- Come with a concept of vehicle
interface IVehicle{
    void start();
    void honk();
}

class car implements Vehicle {

}

Hiding details - abstraction
    - hiding the data (encapsulation) --> means abstraction can be achived by using encapsulation
    - hiding the details into a separate concept/class
Hiding data - encapsulation

class Rectangle {
    private int height;
    private int width;

    int area(){
        return height*width;
    }

    //setters
    void setWidth(int newWidth){
        width = newWidth;
    }
    void setHeight(int newHeight){
        height = newHeight;
    }
    //getters
    int getWidth(){
        return width;
    }
    int getHeighth(){
        return height;
    }
}

class Square extends Rectangle{
    @Override
     void setWidth(int newWidth){
        width = newWidth;
    }
    @Override
    void setHeight(int newHeight){
        //if(newHeight!=width){
          //  throw new InvalidStateException("Height and width must be the same");
        //}
       width = height = newHeight;
    }
}

class Client {
    void foo(){
        Rectangle r = new Rectangle();
        r.setWidth(10);
        r.setHeight(20);
        print(r.area());
    }
}

//getters and setters by default present in java world to make the code backward compatible.
//let's say initially its public and client is direcly using that, tmrw I may have validations 
//on that property and make that field private. In that case, the earlier code will break.
//Hence getters and setters are defacto choice

//SOLID
//S-> SRP --> This is straight forward
//O-> Open for extension and closed for modification
class Rectangle{

}
//If the client wants to add new functionality to the rectangle or they want to provide variations of the rectangle then they should not
//have to modify the rectangle class. Client should be able to extend the functonality by simply using inheritance.

//L-> Liskov substituition principle
//Everything which a parent class can do a child class must be able to do. Runtime polymorphism already enforces that like
Bird b = new Sparrow(); //--> It means whatever actvity a bird can do an sparrow can also do as this sparrow is a bird.
//but people can do stupid things in runtime polymorphism rt
class Bird{
    void fly(){
        print("Flap Wings");
    }
}

class Kiwi extends Bird{
    @override 
    void fly(){
        throw new Exception("No wings, i am extinct");
    }
}

class client {
    void foo(){
        //Bird b = new Bird();//working one
        Bird b = new Kiwi();
        b.fly(); // this will throw exception
    }
}

//LSP says that runtime ploymorphism allows you to do such things but don't do such things. Basically it enforces correctness of the system

//I-> Interface segragation principle
//And the way to solve the above problem is via ISP. ISP says that your iterface should be minimal. Means your your client/subclass should not be forced to implement the functionality which they don't want to support.
class Bird {
    void getBorn();
    void die();
}
interface Flying{
    void fly();
}
class Sparrow extends Bird implements Flying{

}

class Kiwi extends Bird {

}
//Therefore, the way to achieve LSP is via ISP
//D-> Dependency Inversion Principle
//High level code should depend on other high level code and not low level details. Don't create your dependencies yourself. Have your client provide that to you.
class Car {}
class ConsumerCar extends Car {
    //these are dependencies
    ConsumerEngine engine;
    RubberWheel leftWheel;
    GlassWindow window;
    Horn horn;
    void honk(){
        this.horn.honk();
    }
}
class CombatCar extends Car {
    ArmoredEngine engine;
    BulletProofSelfInflatingWheel wheel;
    TaintedBulletProofWindow window;
}
//so, it shouldn't be the case like for these exclusive types as mentioned above, we should be creating different classes. Rather than that, I would prefer
//client should inject th dependency what they want actually rather than relying on low level nitty gritties. Like
class Engine{}
class ConsumerEngine extends Engine {}
class ArmoredEngine extends Engine {}

//high level class
class Car {
    Engine engine;
    Wheel wheel;
    Window window;
    Horn horn;
    //here in ctor, we can use Interface as well
    public Car(Engine engine, Wheel wheel, ...){
        this.engine = engine;
        ....
    }
}

//like wise
class client {
    void foo {
        Car combatCar = new Car(new ArmoredEngine(), new BulletProofSelfInflatingWheel());
    }
}
//so, DIP says that high level code should depend on other high level not on low level details. DIP will be achieved by using abstraction on the 
//dependencies and by using dependency Injection

//Inversion of Control
//You are giving up control.

//classes can have state
//Interfaces can't have any state
//classes can have encapsulation
//interfaces by default are public
//abstract classes --> schema enforcement + some implementation
//interfaces --> schema enforcement

//use interfaces as much as you can.
```