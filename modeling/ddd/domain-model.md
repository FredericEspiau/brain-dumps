# Domain model

At its worst business logic can be very complex. Rules and logic describe many different cases and slants of behavior, and it's this complexity that objects were designed to work with. A Domain Model creates a web of interconnected objects, where each object represents some meaningful individual, whether as large as a corporation or as small as a single line on an order form.

## How It Works

Putting a Domain Model in an application involves inserting a whole layer of objects that model the business area you’re working in. You’ll find objects that mimic the data in the business and objects that capture the rules the business uses. Mostly the data and process are combined to cluster the processes close to the data they work with.
An OO domain model will often look similar to a database model, yet it will still have a lot of differences. A Domain Model mingles data and process, has multivalued attributes and a complex web of associations, and uses inheritance.

As a result I see two styles of Domain Model in the field. A simple Domain Model looks very much like the database design with mostly one domain object for each database table. A rich Domain Model can look different from the data- base design, with inheritance, strategies, and other [Gang of Four] patterns, and complex webs of small interconnected objects. A rich Domain Model is better for more complex logic, but is harder to map to the database. A simple Domain Model can use Active Record (160), whereas a rich Domain Model requires Data Mapper (165).

Since the behavior of the business is subject to a lot of change, it’s important to be able to modify, build, and test this layer easily. As a result you’ll want the minimum of coupling from the Domain Model to other layers in the system. You’ll notice that a guiding force of many layering patterns is to keep as few dependencies as possible between the domain model and other parts of the system.

With a Domain Model there are a number of different scopes you might use. The simplest case is a single-user application where the whole object graph is read from a file and put into memory. A desktop application may work this way, but it’s less common for a multitiered IS application simply because there are too many objects. Putting every object into memory consumes too much memory and takes too long. The beauty of object-oriented databases is that they give the impression of doing this while moving objects between memory and disk.

Without an OO database you have to do this yourself. Usually a session will involve pulling in an object graph of all the objects involved in it. This will certainly not be all objects and usually not all the classes. Thus, if you’re looking at a set of contracts you might pull in only the products referenced by contracts within your working set. If you’re just performing calculations on contracts and revenue recognition objects, you may not pull in any product objects at all. Exactly what you pull into memory is governed by your database mapping objects.

If you need the same object graph between calls to the server, you have to save the server state somewhere, which is the subject of the section on saving server state (page 81).

A common concern with domain logic is bloated domain objects. As you build a screen to manipulate orders you’ll notice that some of the order behavior is only needed for it. If you put these responsibilities on the order, the risk is that the Order class will become too big because it’s full of responsibilities that are only used in a single use case. This concern leads people to consider whether some responsibility is general, in which case it should sit in the order class, or specific, in which case it should sit in some usage-specific class, which might be a Transaction Script (110) or perhaps the presentation itself.

The problem with separating usage-specific behavior is that it can lead to duplication. Behavior that’s separated from the order is harder to find, so people tend to not see it and duplicate it instead. Duplication can quickly lead to more complexity and inconsistency, but I’ve found that bloating occurs much less frequently than predicted. If it does occur, it’s relatively easy to see and not difficult to fix. My advice is not to separate usage-specific behavior. Put it all in the object that’s the natural fit. Fix the bloating when, and if, it becomes a problem.

### Java Implementation

There’s always a lot of heat generated when people talk about developing a Domain Model in J2EE. Many of the teaching materials and introductory J2EE books suggest that you use entity beans to develop a domain model, but there are some serious problems with this approach, at least with the current (2.0) specification.
Entity beans are most useful when you use Container Managed Persistence (CMP). Indeed, I would say there’s little point in using entity beans without CMP. However, CMP is a limited form of object-relational mapping, and it can’t support many of the patterns that you need in a rich Domain Model.
Entity beans can’t be re-entrant. That is, if you call out from one entity bean into another object, that other object (or any object it calls) can’t call back into the first entity bean. A rich Domain Model often uses re-entrancy, so this is a handicap. It’s made worse by the fact that it’s hard to spot re- entrant behavior. As a result, some people say that one entity bean should never call another. While this avoids re-entrancy, it very much cripples the advantages using a Domain Model.
A Domain Model should use fine-grained objects with fine-grained inter- faces. Entity beans may be remotable (prior to version 2.0 they had to be). If you have remote objects with fine-grained interfaces you get terrible performance. You can avoid this problem quite easily by only using local inter- faces for your entity beans in a Domain Model.
To run with entity beans you need a container and a database connected. This will increase build times and also increase the time to do test runs since the tests have to execute against a database. Entity beans are also tricky to debug.
The alternative is to use normal Java objects, although this often causes a surprised reaction—it’s amazing how many people think that you can’t run regular Java objects in an EJB container. I’ve come to the conclusion that people forget about regular Java objects because they haven’t got a fancy name. That’s why, while preparing for a talk in 2000, Rebecca Parsons, Josh Mackenzie, and I gave them one: POJOs (plain old Java objects). A POJO domain model is easy to put together, is quick to build, can run and test out- side an EJB container, and is independent of EJB (maybe that’s why EJB ven- dors don’t encourage you to use them).

My view on the whole is that using entity beans as a Domain Model works if you have pretty modest domain logic. If so, you can build a Domain Model that has a simple relationship with the database: where there’s mostly one entity bean class per database table. If you have a richer domain logic with inheritance, strategies, and other more sophisticated pat- terns, you’re better off with a POJO domain model and Data Mapper (165), using a commercial tool or with a homegrown layer.

The biggest frustration for me with the use of EJB is that I find a rich Domain Model complicated enough to deal with, and I want to keep as inde- pendent as possible from the details of the implementation environment. EJB forces itself into your thinking about the Domain Model, which means that I have to worry about both the domain and the EJB environment.

## When to Use It

If the how for a Domain Model is difficult because it’s such a big subject, the when is hard because of both the vagueness and the simplicity of the advice. It all comes down to the complexity of the behavior in your system. If you have complicated and ever changing business rules involving validation, calculations, and derivations, chances are that you’ll want an object model to handle them. On the other hand, if you have simple not-null checks and a couple of sums to calculate, a Transaction Script (110) is a better bet.

One factor that comes into this is comfortable used the development team is with domain objects. Learning how to design and use a Domain Model is a significant exercise—one that has led to many articles on the “paradigm shift” of objects use. It certainly takes practice and coaching to get used to a Domain Model, but once used to it I’ve found that few people want to go back to a Transaction Script (110) for any but the simplest problems.

If you’re using Domain Model, my first choice for database interaction is Data Mapper (165). This will help keep your Domain Model independent from the database and is the best approach to handle cases where the Domain Model and database schema diverge.

When you use Domain Model you may want to consider Service Layer (133) to give your Domain Model a more distinct API.

## Example: Revenue Recognition (Java)

One of the biggest frustrations of describing a Domain Model is the fact that any example I show is necessarily simple so you can understand it; yet that simplicity hides the Domain Model’s strength. You only appreciate these strengths when you have a really complicated domain.

But even if the example can’t do justice to why you would want a Domain Model, at least it will give you a sense of what one can look like. Therefore, I’m using the same example (page 112) that I used for Transaction Script (110), a little matter of revenue recognition.
An immediate thing to notice is that every class, in this small example (Fig- ure 9.3) contains both behavior and data. Even the humble Revenue Recognition class contains a simple method to find out if that object’s value is recognizable on a certain date.

```java
class RevenueRecognition...
    private Money amount;
    private MfDate date;

    public RevenueRecognition(Money amount, MfDate date) {
        this.amount = amount;
        this.date = date;
    }

    public Money getAmount() {
        return amount;
    }

    boolean isRecognizableBy(MfDate asOf) {
        return asOf.after(date) || asOf.equals(date);
    }
```

Calculating how much revenue is recognized on a particular date involves both the contract and revenue recognition classes.

```java
class Contract...
    private List revenueRecognitions = new ArrayList();
    public Money recognizedRevenue(MfDate asOf) {
        Money result = Money.dollars(0);
        Iterator it = revenueRecognitions.iterator();
        while (it.hasNext()) {
            RevenueRecognition r = (RevenueRecognition) it.next();
            if (r.isRecognizableBy(asOf))
                result = result.add(r.getAmount());
        }
        return result;
    }
```

A common thing you find in domain models is how multiple classes interact to do even the simplest tasks. This is what often leads to the complaint that with OO programs you spend a lot of time hunting around from class to class trying to find them. There’s a lot of merit to this complaint. The value comes as the decision on whether something is recognizable by a certain date gets more complex and as other objects need to know. Containing the behavior on the object that needs to know avoids duplication and reduces coupling between the different objects.

Looking at calculating and creating these revenue recognition objects further demonstrates the notion of lots of little objects. In this case the calculation and creation begin with the customer and are handed off via the product to a strategy hierarchy. The strategy pattern [Gang of Four] is a well-known OO pattern that allows you to combine a group of operations in a small class hierarchy. Each instance of product is connected to a single instance of recognition strategy, which determines which algorithm is used to calculate revenue recognition. In this case we have two subclasses of recognition strategy for the two different cases. The structure of the code looks like this:

```java
class Contract...

    private Product product;
    private Money revenue;
    private MfDate whenSigned;
    private Long id;

    public Contract(Product product, Money revenue, MfDate whenSigned) {
        this.product = product;
        this.revenue = revenue;
        this.whenSigned = whenSigned;
    }

class Product...
    private String name;
    private RecognitionStrategy recognitionStrategy;
    public Product(String name, RecognitionStrategy recognitionStrategy) {
        this.name = name;
        this.recognitionStrategy = recognitionStrategy;
    }
    public static Product newWordProcessor(String name) {
        return new Product(name, new CompleteRecognitionStrategy());
    }

    public static Product newSpreadsheet(String name) {
        return new Product(name, new ThreeWayRecognitionStrategy(60, 90));
    }

    public static Product newDatabase(String name) {
        return new Product(name, new ThreeWayRecognitionStrategy(30, 60));
    }

class RecognitionStrategy...

    abstract void calculateRevenueRecognitions(Contract contract);

class CompleteRecognitionStrategy...
    void calculateRevenueRecognitions(Contract contract) {
        contract.addRevenueRecognition(new RevenueRecognition(contract.getRevenue(), contract.getWhenSigned()));
    }

class ThreeWayRecognitionStrategy...
    private int firstRecognitionOffset;
    private int secondRecognitionOffset;

    public ThreeWayRecognitionStrategy(int firstRecognitionOffset,
    int secondRecognitionOffset)
    {
        this.firstRecognitionOffset = firstRecognitionOffset; this.secondRecognitionOffset = secondRecognitionOffset;
    }

    void calculateRevenueRecognitions(Contract contract) {
        Money[] allocation = contract.getRevenue().allocate(3);
        contract.addRevenueRecognition(new RevenueRecognition
        (allocation[0], contract.getWhenSigned()));
        contract.addRevenueRecognition(new RevenueRecognition
        (allocation[1], contract.getWhenSigned().addDays(firstRecognitionOffset)));
        contract.addRevenueRecognition(new RevenueRecognition
        (allocation[2], contract.getWhenSigned().addDays(secondRecognitionOffset)));
    }
```

The great value of the strategies is that they provide well-contained plug points to extend the application. Adding a new revenue recognition algorithm involves creating a new subclass and overriding the calculateRevenueRecognitions method. This makes it easy to extend the algorithmic behavior of the application.
When you create products, you hook them up with the appropriate strategy objects. I’m doing this in my test code.

```java
class Tester...

    private Product word = Product.newWordProcessor("Thinking Word");
    private Product calc = Product.newSpreadsheet("Thinking Calc");
    private Product db = Product.newDatabase("Thinking DB");
```

Once everything is set up, calculating the recognitions requires no knowledge of the strategy subclasses.

```java
class Contract...
    public void calculateRecognitions() {
        product.calculateRevenueRecognitions(this);
    }

class Product...
    void calculateRevenueRecognitions(Contract contract) {
        recognitionStrategy.calculateRevenueRecognitions(contract);
    }
```

The OO habit of successive forwarding from object to object moves the behavior to the object most qualified to handle it, but it also resolves much of the conditional behavior. You’ll notice that there are no conditionals in this calculation. You set up the decision path when you create the products with the appropriate strategy. Once everything is wired together like this, the algorithms just follow the path. Domain models work very well when you have similar conditionals because the similar conditionals can be factored out into the object structure itself. This moves complexity out of the algorithms and into the relationships between objects. The more similar the logic, the more you find the same network of relationships used by different parts of the system. Any algorithm that’s dependent on the type of recognition calculation can follow this particular network of objects.

Notice in this example that I’ve shown nothing about how the objects are retrieved from, and written to, the database. This is for a couple of reasons. First, mapping a Domain Model to a database is always somewhat hard, so I’m chickening out and not providing an example. Second, in many ways the whole point of a Domain Model is to hide the database, both from upper layers and from people working the Domain Model itself. Thus, hiding it here reflects what it’s like to actually program in this environment.

# Done 

- https://martinfowler.com/eaaCatalog/domainModel.html

---

Because the term domain model includes the word domain, we might get the idea that we should create a single, cohesive, all-inclusive model of an organization’s entire business domain—you know, like an enterprise model. How- ever, when using DDD, that is not our goal. DDD places emphasis on just the opposite. The whole Domain of the organization is composed of Subdomains. Using DDD, models are developed in Bounded Contexts. In fact, developing a Domain Model is actually one way that we focus on only one specific area of the whole business domain. Any attempt to define the business of even a moderately complex organization in a single, all-encompassing model will be at best extremely difficult and will usually fail. As is made clear in this chapter, vigorously separating distinct areas of the whole business domain will help us succeed.
So, if a domain model shouldn’t be all-inclusive of what the organization does and how it does it, what should it be, exactly?