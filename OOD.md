# Object Oriented Design

## Creational Patterns

### Singleton

- ensure a class has only one instance, and provide a global point of access to it
- use case: logger, configuration, connection pool

```cpp
class Singleton {
public:
    static Singleton& getInstance() {
        static Singleton instance;
        return instance;
    }

    void doSomething() {
        std::cout << "Singleton::doSomething()" << std::endl;
    }

private:
    Singleton() {}
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;
};
```

### Factory Method

- define an interface for creating an object, but let subclasses decide which class to instantiate
- instantiating object of classA through classB
- use case: logger (to create different types of loggers)

```cpp
class Product {
public:
    virtual void use() = 0;
    virtual void notUse() = 0;
};

class ConcreteProduct : public Product {
public:
    void use() override {
        std::cout << "ConcreteProduct::use()" << std::endl;
    }
};
```

### Abstract Factory

- provide an interface for creating families of related or dependent objects without specifying their concrete classes
- must override all pure virtual functions from the base class
- use case: GUI library (to create different types of buttons and windows)

```cpp
class AbstractFactory {
public:
    virtual Product* createProduct() = 0;
};

class ConcreteFactory : public AbstractFactory {
public:
    Product* createProduct() override {
        return new ConcreteProduct();
    }
};
```

### Builder

- separate the construction of a complex object from its representation
- construct an object step by step
- use case: Protobuf

```cpp
class Product {
public:
    void setPartA(const std::string& partA) {
        partA_ = partA;
    }

    void setPartB(const std::string& partB) {
        partB_ = partB;
    }

    void setPartC(const std::string& partC) {
        partC_ = partC;
    }

    void show() {
        std::cout << "Product: " << partA_ << ", " << partB_ << ", " << partC_ << std::endl;
    }

private:
    std::string partA_;
    std::string partB_;
    std::string partC_;
};

class Builder {
public:
    virtual void buildPartA() = 0;
    virtual void buildPartB() = 0;
    virtual void buildPartC() = 0;
    virtual Product* getProduct() = 0;
};

class ConcreteBuilder : public Builder {
public:
    void ConcreteBuilder() {
        product_ = Product();
    }

    void buildPartA() override {
        product_->setPartA("A");
    }

    void buildPartB() override {
        product_->setPartB("B");
    }

    void buildPartC() override {
        product_->setPartC("C");
    }

    Product* getProduct() override {
        return product_;
    }

private:
    Product* product_ = new Product();
};

class Director {
public:
    Director(Builder* builder) : builder_(builder) {}

    void construct() {
        builder_->buildPartA();
        builder_->buildPartB();
        builder_->buildPartC();
    }

private:
    Builder* builder_;
};
```

### Prototype

- specify the kinds of objects to create using a prototypical instance, and create new objects by copying this prototype
- think of it as a copy constructor

```cpp
class Prototype {
public:
    virtual Prototype* clone() = 0;
    virtual void show() = 0;
};

class ConcretePrototype : public Prototype {
public:
    Prototype* clone() override {
        return new ConcretePrototype(*this);
    }

    void show() override {
        std::cout << "ConcretePrototype::show()" << std::endl;
    }
};
```

### Static Factory

- use static methods to create objects
- calls factory instead of constructor

```cpp
Foo x = new Foo();
Foo y = Foo.create();
```

## Structural Patterns

### Adapter

- convert the interface of a class into another interface clients expect
- use case: when you have a class that you can't change but you want to use it in your code

### Bridge

- decouple an abstraction from its implementation so that the two can vary independently
- For example, in my Mastermind game, there are different gameplay types
  - so I have a GameType abstract class and it's implemented by SinglePlayerGame and MultiPlayerGame

### Composite

- a way to compare objects and treat them the same as tree-like structures

### Decorator

- creates an interface to be used for adding new features to an object
- interface Coffee -> class PlainCoffee -> abstract class CoffeeDecorator -> class MilkDecorator

### Facade

- i see it as adding another layer of abstraction to simplify the interface of a complex system
- for example, in my mastermind game, i have a class that does all logic for saving, loading, and deleting games but i have another class that just has a method called Save()

### Flyweight

- a way to reduce memory usage or computational expenses
- for example, again in my mastermind game, I used nCurses for terminal UI. Instead of creating a new window for each game state, i just used one window and updated it

### Proxy

## Behavioral Patterns

### Observer

- a way to notify objects of changes, e.g. event listeners, streams, etc.

### Strategy

- a way to define a family of algorithms, encapsulate each one, and make them interchangeable
- in my community data analyzer project, i used this pattern to switch algorithms that analyzes the data to get average property prices and average total livable area per zip code
