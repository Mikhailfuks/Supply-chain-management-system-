using System;
using System.Collections.Generic;
using System.Linq;

public class SupplyChainManagementSystem
{
    private static List<Product> products = new List<Product>()
    {
        new Product("Laptop", 1200, "Electronics"),
        new Product("Smartphone", 800, "Electronics"),
        new Product("Milk", 2.50m, "Grocery"),
        new Product("Bread", 2.00m, "Grocery"),
        new Product("Eggs", 3.00m, "Grocery")
    };

    private static List<Supplier> suppliers = new List<Supplier>()
    {
        new Supplier("TechCorp", "Electronics"),
        new Supplier("DairyFarm", "Grocery")
    };

    private static List<Warehouse> warehouses = new List<Warehouse>()
    {
        new Warehouse("Central Warehouse", "Electronics"),
        new Warehouse("Westside Warehouse", "Grocery")
    };

    private static List<Order> orders = new List<Order>();

    public static void Main(string[] args)
    {
        Console.WriteLine("Welcome to the Supply Chain Management System!");

        while (true)
        {
            Console.WriteLine("nChoose an option:");
            Console.WriteLine("1. View Products");
            Console.WriteLine("2. View Suppliers");
            Console.WriteLine("3. View Warehouses");
            Console.WriteLine("4. Place Order");
            Console.WriteLine("5. View Orders");
            Console.WriteLine("6. Exit");

            int choice = GetIntInput("Enter your choice: ");

            switch (choice)
            {
                case 1:
                    ViewProducts();
                    break;
                case 2:
                    ViewSuppliers();
                    break;
                case 3:
                    ViewWarehouses();
                    break;
                case 4:
                    PlaceOrder();
                    break;
                case 5:
                    ViewOrders();
                    break;
                case 6:
                    Console.WriteLine("Exiting the system. Goodbye!");
                    return;
                default:
                    Console.WriteLine("Invalid choice. Please try again.");
                    break;
            }
        }
    }

    private static void ViewProducts()
    {
        Console.WriteLine("nAvailable Products:");
        if (products.Count == 0)
        {
            Console.WriteLine("No products available.");
        }
        else
        {
            foreach (Product product in products)
            {


                Console.WriteLine($"{product.Name} - ${product.Price} - Category: {product.Category}");
            }
        }
    }

    private static void ViewSuppliers()
    {
        Console.WriteLine("\nSuppliers:");
        if (suppliers.Count == 0)
        {
            Console.WriteLine("No suppliers registered.");
        }
        else
        {
            foreach (Supplier supplier in suppliers)
            {
                Console.WriteLine($"{supplier.Name} - Category: {supplier.Category}");
            }
        }
    }

    private static void ViewWarehouses()
    {
        Console.WriteLine("\nWarehouses:");
        if (warehouses.Count == 0)
        {
            Console.WriteLine("No warehouses registered.");
        }
        else
        {
            foreach (Warehouse warehouse in warehouses)
            {
                Console.WriteLine($"{warehouse.Name} - Category: {warehouse.Category}");
            }
        }
    }

    private static void PlaceOrder()
    {
        Console.WriteLine("\nPlacing Order");

        Console.Write("Enter product name: ");
        string productName = Console.ReadLine();

        Product product = products.FirstOrDefault(p => p.Name.Equals(productName, StringComparison.OrdinalIgnoreCase));

        if (product != null)
        {
            Console.Write("Enter quantity: ");
            int quantity = GetIntInput("");

            // Find a suitable supplier and warehouse
            Supplier supplier = suppliers.FirstOrDefault(s => s.Category == product.Category);
            Warehouse warehouse = warehouses.FirstOrDefault(w => w.Category == product.Category);

            if (supplier != null && warehouse != null)
            {
                Order newOrder = new Order(product, quantity, supplier, warehouse);
                orders.Add(newOrder);
                Console.WriteLine("Order placed successfully!");
                Console.WriteLine($"Order ID: {newOrder.Id}");
                Console.WriteLine($"Supplier: {supplier.Name}");
                Console.WriteLine($"Warehouse: {warehouse.Name}");
            }
            else
            {
                Console.WriteLine("No suitable supplier or warehouse found for this product.");
            }
        }
        else
        {


            Console.WriteLine("Product not found.");
        }
    }

    private static void ViewOrders()
    {
        Console.WriteLine("\nPlaced Orders:");
        if (orders.Count == 0)
        {
            Console.WriteLine("No orders placed yet.");
        }
        else
        {
            foreach (Order order in orders)
            {
                Console.WriteLine($"Order ID: {order.Id}");
                Console.WriteLine($"Product: {order.Product.Name} - Quantity: {order.Quantity}");
                Console.WriteLine($"Supplier: {order.Supplier.Name}");
                Console.WriteLine($"Warehouse: {order.Warehouse.Name}");
                Console.WriteLine("---------------------");
            }
        }
    }

    // Helper method to get integer input from the user
    private static int GetIntInput(string prompt)
    {
        while (true)
        {
            Console.Write(prompt);
            if (int.TryParse(Console.ReadLine(), out int input))
            {
                return input;
            }
            Console.WriteLine("Invalid input. Please enter a number.");
        }
    }
}

// Product class
public class Product
{
    public string Name { get; set; }
    public decimal Price { get; set; }
    public string Category { get; set; }

    public Product(string name, decimal price, string category)
    {
        Name = name;
        Price = price;
        Category = category;
    }
}


