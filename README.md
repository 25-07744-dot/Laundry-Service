# Laundry Pickup & Delivery System

**Description:** "Laundry Pickup & Delivery System" is a console-based laundry scheduling and order management program that lets users view available laundry services, create pickup and delivery schedules, calculate base and express fees, generate receipts, and review recorded orders. The program demonstrates core object-oriented programming principles using an abstract superclass, specialized service subclasses, encapsulated data models, and polymorphic cost calculation.

## Object-Oriented Programming Concepts Applied

The following are the detailed documentation of how the OOP principles are applied throughout the program:

### Encapsulation

```java
abstract class LaundryService {
    private String serviceName;
    private double pricePerKg;

    public LaundryService(String serviceName, double pricePerKg) {
        this.serviceName = serviceName;
        this.pricePerKg = pricePerKg;
    }

    public String getServiceName() {
        return serviceName;
    }

    public double getPricePerKg() {
        return pricePerKg;
    }

    public abstract double calculateCost(double weight);
}
```
In `LaundryService`, `serviceName` and `pricePerKg` are declared `private` and accessed through public getter methods (`getServiceName()` and `getPricePerKg()`), protecting service details from direct external modification while allowing subclasses to read their rates.

```java
class Customer {
    private String customerName;

    public Customer(String customerName) {
        this.customerName = customerName;
    }

    public String getCustomerName() {
        return customerName;
    }
}
```
The `Customer` class encapsulates `customerName` as a `private` field and exposes it only through `getCustomerName()`.

```java
class LaundryOrder {
    private Customer customer;
    private LaundryService service;
    private double weight;
    private LocalDate pickupDate;
    private LocalDate deliveryDate;
    private boolean express;
```
`LaundryOrder` keeps its order details (`customer`, `service`, `weight`, `pickupDate`, `deliveryDate`, and `express`) `private` and computes billing amounts through controlled methods (`getBaseCost()`, `getExpressFee()`, `getTotalCost()`, and `displayReceipt()`).

### Inheritance

```java
class WashFold extends LaundryService {
    public WashFold() {
        super("Wash & Fold", 35.00);
    }

    @Override
    public double calculateCost(double weight) {
        return getPricePerKg() * weight;
    }
}
```
Each specific laundry service (`WashFold`, `WashIron`, `DryCleaning`, `Bedding`, and `StainRemoval`) extends `LaundryService` using `super(...)` to inherit `serviceName`, `pricePerKg`, and their getters without duplicating field declarations.

### Abstraction

```java
public abstract double calculateCost(double weight);
```
`LaundryService` is an `abstract` class that declares `calculateCost(double weight)` as an abstract method, hiding how each service calculates its rate while requiring every subclass to provide its own implementation. In `Main`, input validation and parsing helpers (`readInt()`, `readDouble()`, `readDate()`) also hide parsing and error-handling details from the main menu flow.

### Polymorphism

```java
@Override
public double calculateCost(double weight) {
    return getPricePerKg() * weight;
}
```
Each subclass of `LaundryService` (`WashFold` at PHP 35.00/kg, `WashIron` at PHP 55.00/kg, `DryCleaning` at PHP 120.00/kg, `Bedding` at PHP 80.00/kg, and `StainRemoval` at PHP 95.00/kg) overrides `calculateCost(double weight)`. Because `Main` stores services in `ArrayList<LaundryService>`, calling `service.calculateCost(weight)` dynamically invokes the selected service's implementation at runtime.

## Program Structure

### LaundryService Class (Abstract)
**ROLE:**
- Serves as the base abstract class for all laundry services
- Stores common service name and per-kilogram pricing

**RESPONSIBILITIES:**
- `getServiceName()`
- `getPricePerKg()`
- `calculateCost(double weight)` (abstract)

**RELATIONSHIPS:**
- Superclass of `WashFold`, `WashIron`, `DryCleaning`, `Bedding`, and `StainRemoval`
- Referenced by `LaundryOrder` and `Main`

### Service Subclasses (WashFold, WashIron, DryCleaning, Bedding, StainRemoval)
**ROLE:**
- Represent individual laundry service offerings with their respective per-kilogram rates

**DATA:**
- Inherited `serviceName` and `pricePerKg` initialized via `super(...)`

**RESPONSIBILITIES:**
- Provide service-specific pricing (`35.00`, `55.00`, `120.00`, `80.00`, `95.00` PHP/kg)
- Override `calculateCost(double weight)`

**RELATIONSHIPS:**
- Extend `LaundryService`

### Customer Class
**ROLE:**
- Stores customer identification details for a laundry order

**DATA:**
- `customerName`

**RESPONSIBILITIES:**
- Store and return the customer's name via `getCustomerName()`

**RELATIONSHIPS:**
- Associated with `LaundryOrder`

### LaundryOrder Class
**ROLE:**
- Represents a scheduled laundry order, calculates charges, and prints the receipt

**DATA:**
- `customer`
- `service`
- `weight`
- `pickupDate`
- `deliveryDate`
- `express`

**RESPONSIBILITIES:**
- `getBaseCost()`
- `getExpressFee()` (adds a 20% fee when `express` is true)
- `getTotalCost()`
- `displayReceipt()`

**RELATIONSHIPS:**
- Has-a `Customer` and a `LaundryService`
- Stored in `orders` inside `Main`

### Main Class
**ROLE:**
- Serves as the entry point and console controller for the Laundry Pickup & Delivery System

**DATA STRUCTURES:**
- `ArrayList<LaundryService> services` — Stores initialized laundry services
- `ArrayList<LaundryOrder> orders` — Stores created laundry orders in memory

**RESPONSIBILITIES:**
- `initializeServices()`, `displayMenu()`, `displayServices()`, `createOrder()`, `displayAllOrders()`
- Input validation helpers: `readInt()`, `readDouble()`, `readDate()`

**RELATIONSHIPS:**
- Manages `LaundryService` subclasses, `Customer`, and `LaundryOrder` objects

## How to Run the Program

### PREREQUISITES
1. Before running the program, make sure Java Development Kit (JDK) is installed (Java `java.time.*` classes require JDK 8 or later). Open your terminal and check the installation:

```bash
java -version
javac -version
```
   - If you see an error like `java is not recognized`, Java is not installed or not added to your system's `PATH`. Download the JDK from: https://www.oracle.com/java/technologies/downloads/
2. Go to the GitHub repository.
3. Click the green **Code** button and select **Download ZIP**.
4. Extract the ZIP file to a folder on your computer.

### AFTERWARDS, COMPILE THE PROGRAM
5. Open VS Code or another Java-supported IDE.
6. Select **File**, then choose **Open Folder**, and select the project folder.
7. Open `Main.java` and run the program or open a terminal in the project folder and compile/run:

```bash
javac Main.java
java Main
```

### TO NAVIGATE THE PROGRAM

1. **Main Menu**
   - `[1] Create Laundry Schedule` — Starts creating a new laundry order
   - `[2] View Available Services` — Lists all 5 services and their PHP per-kg rates
   - `[3] View All Orders` — Displays all recorded orders and their receipts
   - `[4] Exit` — Exits the program

2. **Create Laundry Schedule**
   - Enter a non-empty customer name.
   - Select a service from `1` to `5`.
   - Enter laundry weight above `0` and up to `50` kg.
   - Enter Pickup Date and Delivery Date in `MM-DD-YYYY` format (delivery date must be after pickup date).
   - Choose Express Order (`Y` for +20% fee or `N` for standard).
   - Displays the generated receipt and saves the order to the list.

## Sample Output

1. **Main Menu**
   Displays the available options for the Laundry Pickup & Delivery System.
   `[
   <img width="828" height="463" alt="444bc69e-ca87-4737-a7ab-9efffa74d7dd" src="https://github.com/user-attachments/assets/95faea98-0c61-4534-8faf-63ce0eb3d597" />
]'


3. **Available Services**
   Displays the list of services and their rates per kilogram.
   '[
   <img width="828" height="767" alt="c58c1a75-8850-4955-a47a-35115f3997a6" src="https://github.com/user-attachments/assets/8e887faf-1de1-4297-a592-55563edd4c5b" />
]'

5. **Create Laundry Schedule**
   Prompts for customer name, service selection, weight, pickup/delivery dates, and express option.
   '[
   <img width="828" height="842" alt="8515ec10-64bd-451f-816a-1f2ff208c4d1" src="https://github.com/user-attachments/assets/c136f3d8-29c7-43b8-a7f7-941191962151" />
]'

7. **Laundry Schedule Receipt**
   Displays the formatted receipt with base cost, express fee, and total amount due.
   '[
   <img width="828" height="1178" alt="b3e97c03-f69e-41ef-b200-60eb05fde076 (2)" src="https://github.com/user-attachments/assets/e5708bc4-6886-4684-bdf5-1fa62578a17f" />
]'

9. **View All Orders**
   Displays all recorded orders and total order count.
   '[
   <img width="828" height="842" alt="8515ec10-64bd-451f-816a-1f2ff208c4d1" src="https://github.com/user-attachments/assets/7cf10937-0a65-481b-8e1c-4e5c8e27ec68" />
]'

## Author and Acknowledgement

This program is created by `[Insert Group Name]` from `IT - 2110` in compliance with the final project requirement in the course Object-Oriented Programming.

Lead Programmer:
`Marquinez, Zuzannha Jhane M.`

Members:
`Bacaraman, Jawad R.`
`Comendador, Kyle Krizel C.`


Special thanks to `Mr. Dominic Valdez` for guidance and introducing the OOP concepts used in this project.

## Future Enhancements

Future improvements may include saving orders and service rates to a file or database so data persists after closing the program, adding order status tracking (such as scheduled, picked up, washing, and delivered), supporting custom discounts or promos, and building a graphical user interface (GUI).


