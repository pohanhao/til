# Observer pattern

create a pojo class(Anamial)
```java
public class Animal {
    private String name;
    public Animal (String name) {
        this.name = name;
    }
    public String getName () {
        return this.name;
    }
    public void setName (String name) {
        this.name = name;
    }
}
```

observer interface
```java
public interface AnimalAddedListener {
    public void onAnimalAdded (Animal animal);
}
```

subject concrete class
```java
public class Zoo {
    private List<Animal> animals = new ArrayList<>();
    private List<AnimalAddedListener> listeners = new ArrayList<>();
    
    public void addAnimal (Animal animal) {
        // Add the animal to the list of animals
        this.animals.add(animal);
        // Notify the list of registered listeners
        this.notifyAnimalAddedListeners(animal);
    }
    public void registerAnimalAddedListener (AnimalAddedListener listener) {
        // Add the listener to the list of registered listeners
        this.listeners.add(listener);
    }
    public void unregisterAnimalAddedListener (AnimalAddedListener listener) {
        // Remove the listener from the list of the registered listeners
        this.listeners.remove(listener);
    }
    protected void notifyAnimalAddedListeners (Animal animal) {
        // Notify each of the listeners in the list of registered listeners
        this.listeners.forEach(listener -> listener.updateAnimalAdded(animal));
    }
}
```

observer concrete class
```java
public class PrintNameAnimalAddedListener implements AnimalAddedListener {
    @Override
    public void updateAnimalAdded (Animal animal) {
        // Print the name of the newly added animal
        System.out.println("Added a new animal with name '" + animal.getName() + "'");
    }
}
```

demo main
```java
public class Main {
    public static void main (String[] args) {
        // Create the zoo to store animals
        Zoo zoo = new Zoo();
        // Register a listener to be notified when an animal is added
        zoo.registerAnimalAddedListener(new PrintNameAnimalAddedListener());
        // Add an animal notify the registered listeners
        zoo.addAnimal(new Animal("Tiger"));
    }
}
```

output
```
Added a new animal with name 'Tiger'
```


**Reference**

1. https://dzone.com/articles/the-observer-pattern-using-modern-java
2. https://blog.jason.party/10/observer-pattern
