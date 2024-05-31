public class Product {
    private String name;
    private double price;
    private int stock;

    public Product(String name, double price, int stock) {
        this.name = name;
        this.price = price;
        this.stock = stock;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    public int getStock() {
        return stock;
    }

    public void setStock(int stock) {
        this.stock = stock;
    }
    
    @Override
    public String toString() {
        return name + " - $" + price + " - Stock: " + stock;
    }
}

-------------
public class Sale {
    private Product product;
    private int quantity;
    private double total;

    public Sale(Product product, int quantity) {
        this.product = product;
        this.quantity = quantity;
        this.total = product.getPrice() * quantity;
    }

    public Product getProduct() {
        return product;
    }

    public int getQuantity() {
        return quantity;
    }

    public double getTotal() {
        return total;
    }
}

-------------------
import java.util.ArrayList;
import java.util.List;

public class ProductController {
    private List<Product> products;

    public ProductController() {
        this.products = new ArrayList<>();
    }

    public void addProduct(Product product) {
        products.add(product);
    }

    public void removeProduct(Product product) {
        products.remove(product);
    }

    public List<Product> getProducts() {
        return products;
    }

    public Product findProductByName(String name) {
        for (Product product : products) {
            if (product.getName().equals(name)) {
                return product;
            }
        }
        return null;
    }
}

---------------------
import java.util.ArrayList;
import java.util.List;

public class SaleController {
    private List<Sale> sales;

    public SaleController() {
        this.sales = new ArrayList<>();
    }

    public void addSale(Sale sale) {
        sales.add(sale);
    }

    public List<Sale> getSales() {
        return sales;
    }

    public double getTotalSales() {
        return sales.stream().mapToDouble(Sale::getTotal).sum();
    }
}

-----------------------
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class ProductFrame extends JFrame {
    private JTextField nameField;
    private JTextField priceField;
    private JTextField stockField;
    private JButton addButton;
    private ProductController controller;

    public ProductFrame(ProductController controller) {
        this.controller = controller;

        setTitle("Product Management");
        setSize(300, 200);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        JPanel panel = new JPanel();
        panel.setLayout(new GridLayout(4, 2));

        panel.add(new JLabel("Name:"));
        nameField = new JTextField();
        panel.add(nameField);

        panel.add(new JLabel("Price:"));
        priceField = new JTextField();
        panel.add(priceField);

        panel.add(new JLabel("Stock:"));
        stockField = new JTextField();
        panel.add(stockField);

        addButton = new JButton("Add Product");
        panel.add(addButton);

        add(panel);

        addButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String name = nameField.getText();
                double price = Double.parseDouble(priceField.getText());
                int stock = Integer.parseInt(stockField.getText());
                Product product = new Product(name, price, stock);
                controller.addProduct(product);
                JOptionPane.showMessageDialog(null, "Product added!");
            }
        });
    }
}

