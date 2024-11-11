# ironhack
Design Problem Solving
___________________________

Global Configuration Management

Respuesta: Singleton
El patrón Singleton garantiza que exista solo una instancia y proporciona un punto de acceso unico y global a esta instancia. Este patrón es ideal para manejar la configuración global de una aplicación

public class ConfigurationManager {
    private static ConfigurationManager instance;
    private Properties properties;

    private ConfigurationManager() {
        properties = new Properties();
        loadProperties();
    }

    public static synchronized ConfigurationManager getInstance() {
        if (instance == null) {
            instance = new ConfigurationManager();
        }
        return instance;
    }

    private void loadProperties() {
    }

    public String getProperty(String key) {
        return properties.getProperty(key);
    }

    public void setProperty(String key, String value) {
        properties.setProperty(key, value);
    }
}


________________________________

Dynamic Object Creation Based on User Input

Respuesta: Factory
el patrón Factory crea objetos sin especificar la clase exacta del objeto que se creará. Este patrón es ideal para crear dinámicamente varios tipos de elementos de interfaz de usuario según las acciones del usuario

public interface Animal {
        void makeSound();
    }
    
    public class Dog implements Animal {
        @Override
        public void makeSound() {
            System.out.println("Woof!");
        }
    }
    
    public class Cat implements Animal {
        @Override
        public void makeSound() {
            System.out.println("Meow!");
        }
    }

    public class AnimalFactory {
        public static Animal createAnimal(String type) {
            switch (type) {
                case "dog":
                    return new Dog();
                case "cat":
                    return new Cat();
                default:
                    throw new IllegalArgumentException("Unknown animal type");
            }
        }
    }

_________________________________________

State Change Notification Across System Components

Respuesta: Observer
El patrón Observer permite que un objeto notifique a otros objetos sobre cambios en su estado sin crear una fuerte dependencia entre ellos. Este patrón es ideal para notificar a los componentes del sistema sobre cambios de estado en otras partes del sistema

    interface Observer {
        void update(String state);
    }

    class ConcreteObserver implements Observer {
        private String name;
    
        public ConcreteObserver(String name) {
            this.name = name;
        }
    
        @Override
        public void update(String state) {
            System.out.println(name + " received state change: " + state);
        }
    }

    class Subject {
        private List<Observer> observers = new ArrayList<>();
        private String state;
    
        public void addObserver(Observer observer) {
            observers.add(observer);
        }
    
        public void removeObserver(Observer observer) {
            observers.remove(observer);
        }
    
        public void setState(String state) {
            this.state = state;
            notifyObservers();
        }
    
        private void notifyObservers() {
            for (Observer observer : observers) {
                observer.update(state);
            }
        }
    }


__________________________________________

Efficient Management of Asynchronous Operations


Respuesta: Future/Promise
El patrón Future/Promise se utiliza para gestionar operaciones asíncronas y coordinar múltiples llamadas API sin bloquear el flujo principal de la aplicación

import java.util.concurrent.CompletableFuture;

public class AsyncManager {
    public CompletableFuture<String> fetchDataFromAPI(String apiUrl) {
        return CompletableFuture.supplyAsync(() -> {
            // Simulate API call
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            return "Data from " + apiUrl;
        });
    }

    public static void main(String[] args) {
        AsyncManager manager = new AsyncManager();

        CompletableFuture<String> future1 = manager.fetchDataFromAPI("http://api1.com");
        CompletableFuture<String> future2 = manager.fetchDataFromAPI("http://api2.com");

        CompletableFuture<Void> combinedFuture = CompletableFuture.allOf(future1, future2);

        combinedFuture.thenRun(() -> {
            try {
                System.out.println(future1.get());
                System.out.println(future2.get());
            } catch (Exception e) {
                e.printStackTrace();
            }
        });
    }
}
