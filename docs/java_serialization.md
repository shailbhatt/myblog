#Workaround for Java Deeply Flawed Serialization

![](./java_serialization/java_serialization.webp)

> 
 As Brian Goetz, Java language architect, puts it: “Serialization constitutes an invisible but public constructor, and an invisible but public set of accessors for your internal state.”

Let me give you an example about what is the wrong with the Java Serialization.

Suppose you have a class with the valid parameterized constructor as below.

```
public class Car {
     private String name;
    private double x;
    private double y;
    
    protected AudiCar(double x, double y, String name) {
        this.x = x;
        this.y = y;
        this.name = name;
    }
    
    public String getName() { return name; }
    public double getX() { return x; }
    public double getY() { return y; }
}
``` 
Now lets extends this class. For an example

```
public class AudiCar extends Car implements Serializable {

    public AudiCar(double x, double y, String name) {
        super(x, y, name);
    }

    ...
}
```

Now if you try to serialize and the deserialize this class you will see the beautiful exception saying "InvalidClassCastException". here your console is complaining about the no valid constructor.  I am sure you have faced this kinda hopeless situation either while deserializing SQS message to java class or in some other situation. 
What do we generally end up doing here? I have seen people remembering  Lombork and just added @NoArgsConstructor annotation or add the public no agrument constructor and resolved the situation. Then later writer about deserializes object validation method to check deserialized object state is valid or not. This is the flaw I am talking about.

Now to handle this Java Serialization Flaw, there are two ways,
1. Serialization Proxy Pattern
2. Use Records (feature of Java14)

Lets see Serialization Proxy Pattern,

a. you need to add following inner class to the AudiCar class as below
b. you need to add writeReplace method into AudiCar class as below.

```
import java.io.InvalidObjectException;
import java.io.ObjectInputStream;
import java.io.Serializable;

public class AudiCar extends Car implements Serializable {
    private String name;
    private double x;
    private double y;

    protected AudiCar(double x, double y, String name) {
        this.x = x;
        this.y = y;
        this.name = name;
    }

    public String getName() { return name; }
    public double getX() { return x; }
    public double getY() { return y; }

    private Object writeReplace() {
        return new SerializationProxy(this);
    }

   private void readObject(ObjectInputStream stream) throws InvalidObjectException {
        throw new InvalidObjectException("Use Serialization Proxy instead.");
    }

    private static class SerializationProxy implements Serializable {
        private String name;
        private double x;
        private double y;
        public SerializationProxy(AudiCar audiCar) {
            this.name = audiCar.getName();
            this.x = audiCar.getX();
            this.y = audiCar.getY();
        }

        private Object readResolve() {
            return new AudiCar(x, y, name);
        }
    }
}
```
Now when AudiCar class instance will be serialize it will call to writeReplace method. Because of that SerializationProxy class will actually gets serialize instead of the AudiCar class. 

Simillarly, when SerializationProxy class will get deserialize, it will call to readResolve method. As per the above example, readResolve() will create new AudiCar instance while deserializing.

As you see, we've made AudiCar serializable, regardless of non agrument constructor of non-serializable 'Car' parent class.

If you will wonder why I have added readObject method.....! It will prevent deserialization of the enclosing class.

2. Checkout Records  [here](https://blogs.oracle.com/javamagazine/records-come-to-java#anchor_4 ) .

I will cover Java Records in details in upcoming articles. 

If you liked my post, share and like or you can message me on twitter @shail_bhattt for any kind of the discussion. Happy Learning!
Checkout my twitter if you like to consume Java related topics. 