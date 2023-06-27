product java
public class Product {

    private String productName;

    private String productDescription;

    private double productPrice;

    public Product(){

    }

    public Product(String productName, String productDescription, double productPrice) {
        this.productName = productName;
        this.productDescription = productDescription;
        this.productPrice = productPrice;
    }

    public String getProductName() {
        return productName;
    }

    public void setProductName(String productName) {
        this.productName = productName;
    }

    public String getProductDescription() {
        return productDescription;
    }

    public void setProductDescription(String productDescription) {
        this.productDescription = productDescription;
    }

    public double getProductPrice() {
        return productPrice;
    }

    public void setProductPrice(double productPrice) {
        this.productPrice = productPrice;
    }
}
















main.java
import java.sql.*;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.function.Function;
import java.util.stream.Collectors;

public class Main {

    private static Connection con;

    public static void main(String[] args) throws SQLException {

        Product p1 = new Product("Dzexvi", "kargi dzexvi", 12);
        Product p2 = new Product("Ketchupi", "witeli ketchupi", 12);
        Product p3 = new Product("Xorci", "dzroxis xorci", 12);


        List<Product> productList = new ArrayList<>();

        for (int i = 0; i < 4; i ++){
            productList.add(p1);
            productList.add(p2);
            productList.add(p3);
        }

        Map<Product, Long> productQuantityMap = productList.stream()
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

        con = DriverManager.getConnection("jdbc:mysql://localhost:3306/myDb", "user1", "pass");
        initializeDB();

    }


    public static void initializeDB() {
        try (Statement stmt = con.createStatement()) {
            // use stmt here
            String tableSql = "CREATE TABLE IF NOT EXISTS products"
                    + "(product_name varchar(30), product_description varchar(30),"
                    + "product_price double)";
            stmt.execute(tableSql);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    public void addProduct(String productName, String productDescription, double productPrice) {
        Product p = new Product(productName, productDescription, productPrice);
        String insertSql = "INSERT INTO products(product_name, product_description, product_price)"
                + " VALUES('?', '?', ?)";
        try (PreparedStatement stmt = con.prepareStatement(insertSql)) {
            stmt.setString(1, productName);
            stmt.setString(2, productDescription);
            stmt.setDouble(3, productPrice);
            stmt.executeUpdate(insertSql);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    public List<Product> getAllProducts() throws SQLException {
        List<Product> resList = new ArrayList<>();
        String selectSql = "SELECT * FROM products";
        PreparedStatement ps = con.prepareStatement(selectSql);
        ResultSet rs = ps.executeQuery();
        while ( rs.next() ){
            Product p = new Product(rs.getString(1), rs.getString(2), rs.getDouble(3));
            resList.add(p);
        }
        return resList;
    }


}


main.class
ublic class Main {
    public Main() { /* compiled code */ }

    public static void main(java.lang.String[] args) { /* compiled code */ }
}

product.class
public class Product {
    private java.lang.String productName;
    private java.lang.String productDescription;
    private double productPrice;

    public Product() { /* compiled code */ }

    public Product(java.lang.String productName, java.lang.String productDescription, double productPrice) { /* compiled code */ }

    public java.lang.String getProductName() { /* compiled code */ }

    public void setProductName(java.lang.String productName) { /* compiled code */ }

    public java.lang.String getProductDescription() { /* compiled code */ }

    public void setProductDescription(java.lang.String productDescription) { /* compiled code */ }

    public double getProductPrice() { /* compiled code */ }

    public void setProductPrice(double productPrice) { /* compiled code */ }
