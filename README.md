# Garage-billing-app


import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Scanner;

// 1st class: Customer
class Customer {
    private String name;
    private String phone;
    private Car car;

    public Customer(String name, String phone, Car car) {
        this.name = name;
        this.phone = phone;
        this.car = car;
    }

    public String getName() {
        return name;
    }

    public String getPhone() {
        return phone;
    }

    public Car getCar() {
        return car;
    }
}

// 2nd class: Car
class Car {
    private String carNumber;
    private String model;

    public Car(String carNumber, String model) {
        this.carNumber = carNumber;
        this.model = model;
    }

    public String getCarNumber() {
        return carNumber;
    }

    public String getModel() {
        return model;
    }
}

// 3rd class: Service
class Service {
    private String name;
    private double price;

    public Service(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public double getPrice() {
        return price;
    }
}

// 4th class: Invoice
class Invoice {
    private Customer customer;
    private double totalAmount;
    private List<Service> serviceList;

    public Invoice(Customer customer) {
        this.customer = customer;
        this.serviceList = new ArrayList<>();
        this.totalAmount = 0;
    }

    public void addService(Service service) {
        serviceList.add(service);
        totalAmount += service.getPrice();
    }

    public void printInvoice() {
        System.out.println("\n--- Invoice ---");
        System.out.println("Customer Name: " + customer.getName() + "   Phone: " + customer.getPhone());
        System.out.println("Car Model: " + customer.getCar().getModel() + "   Car Number: " + customer.getCar().getCarNumber());
        System.out.println("Services:");
        for (Service service : serviceList) {
            System.out.println(" - " + service.getName() + " : ₹" + service.getPrice());
        }
        System.out.println("Total Amount: ₹" + totalAmount);
        System.out.println("--- Thank you! ---\n");
    }
}

// 5th class: GarageService
class GarageService {
    private List<Service> availableService;
    private Map<String, Customer> customersMap;

    public GarageService() {
        this.availableService = new ArrayList<>();
        this.customersMap = new HashMap<>();
        loadServices();
    }

    private void loadServices() {
        availableService.add(new Service("Washing", 300));
        availableService.add(new Service("Tyre Exchange", 3000));
        availableService.add(new Service("Oil Change", 700));
        availableService.add(new Service("Wheel Alignment", 500));
    }

    public void addCustomer(String name, String phone, String carNumber, String model) {
        Car car = new Car(carNumber, model);
        Customer customer = new Customer(name, phone, car);
        customersMap.put(carNumber, customer);
        System.out.println("Customer added successfully.\n");
    }

    public void createInvoice(String carNumber) {
        if (!customersMap.containsKey(carNumber)) {
            System.out.println("No customer found with car number: " + carNumber);
            return;
        }

        Scanner sc = new Scanner(System.in);
        Customer customer = customersMap.get(carNumber);
        Invoice invoice = new Invoice(customer);

        System.out.println("Available Services:");
        for (int i = 0; i < availableService.size(); i++) {
            System.out.println((i + 1) + " . " + availableService.get(i).getName() + " - ₹ " + availableService.get(i).getPrice());
        }

        while (true) {
            System.out.print("Enter service number to add (or 0 to finish): ");
            int choice = sc.nextInt();
            if (choice == 0) break;

            if (choice > 0 && choice <= availableService.size()) {
                invoice.addService(availableService.get(choice - 1));
                System.out.println("Service added.");
            } else {
                System.out.println("Invalid choice. Try again.");
            }
        }

        invoice.printInvoice();
    }
}

// 6th class: Main App
public class CarGarageBillingApp {
    public static void main(String[] args) {
        GarageService garageService = new GarageService();
        Scanner sc = new Scanner(System.in);

        System.out.println("--- Bharti Garage Service Center ---");

        while (true) {
            System.out.println("\n1. Add Customer");
            System.out.println("2. Create Invoice");
            System.out.println("3. Exit");
            System.out.print("Enter your choice: ");

            int choice = sc.nextInt();

            switch (choice) {
                case 1:
                    System.out.print("Enter customer name: ");
                    String name = sc.next();
                    System.out.print("Enter phone number: ");
                    String phone = sc.next();
                    System.out.print("Enter car number: ");
                    String carNumber = sc.next();
                    System.out.print("Enter car model: ");
                    String model = sc.next();
                    garageService.addCustomer(name, phone, carNumber, model);
                    break;

                case 2:
                    System.out.print("Enter car number: ");
                    String carNo = sc.next();
                    garageService.createInvoice(carNo);
                    break;

                case 3:
                    System.out.println("Exiting... Thank you!");
                    sc.close();
                    return;

                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }
}
