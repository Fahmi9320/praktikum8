# praktikum8
## Nama  : Fahmi Arifin
## Nim   : 312310662
## Kelas : TI.23.A.6
# Kode
```
import java.text.NumberFormat;
import java.util.Date;
import java.util.Locale;

// Class Customer
class Customer {
    private String name;
    private String address;

    public Customer(String name, String address) {
        this.name = name;
        this.address = address;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}

// Class Item
class Item {
    public Item(float shippingWeight, String description) {
    }

    public float getPriceForQuantity(int quantity) {
        return quantity * 100000; // Harga contoh dalam Rupiah (per unit: Rp100.000)
    }

    public float getTax() {
        return 0.1f; // Pajak 10%
    }

    public boolean inStock() {
        return true;
    }
}

// Class Order
class Order {
    public Order(Date date, String status) {
    }

    public float calcSubTotal(OrderDetail[] details) {
        float subTotal = 0;
        for (OrderDetail detail : details) {
            subTotal += detail.calcSubTotal();
        }
        return subTotal;
    }

    public float calcTax(OrderDetail[] details) {
        float tax = 0;
        for (OrderDetail detail : details) {
            tax += detail.calcTax();
        }
        return tax;
    }

    public float calcTotal(OrderDetail[] details) {
        return calcSubTotal(details) + calcTax(details);
    }

    public float calcTotalWeight(OrderDetail[] details) {
        float totalWeight = 0;
        for (OrderDetail detail : details) {
            totalWeight += detail.calcWeight();
        }
        return totalWeight;
    }
}

// Class OrderDetail
class OrderDetail {
    private int quantity;
    private Item item;

    public OrderDetail(int quantity, String status, Item item) {
        this.quantity = quantity;
        this.item = item;
    }

    public float calcSubTotal() {
        return item.getPriceForQuantity(quantity);
    }

    public float calcWeight() {
        return quantity * 1.5f; // Contoh berat per unit
    }

    public float calcTax() {
        return calcSubTotal() * item.getTax();
    }
}

// Abstract Class Payment
abstract class Payment {
    protected float amount;

    public Payment(float amount) {
        this.amount = amount;
    }

    public abstract boolean authorized();
}

// Class Cash
class Cash extends Payment {
    private float cashTendered;

    public Cash(float amount, float cashTendered) {
        super(amount);
        this.cashTendered = cashTendered;
    }

    @Override
    public boolean authorized() {
        return cashTendered >= amount;
    }
}

// Class Check
class Check extends Payment {
    private String bankID;

    public Check(float amount, String name, String bankID) {
        super(amount);
        this.bankID = bankID;
    }

    @Override
    public boolean authorized() {
        return bankID != null && !bankID.isEmpty();
    }
}

// Class Credit
class Credit extends Payment {
    private Date expDate;

    public Credit(float amount, String number, String type, Date expDate) {
        super(amount);
        this.expDate = expDate;
    }

    @Override
    public boolean authorized() {
        return expDate.after(new Date());
    }
}

// Public Class with Main Method
public class OrderManagementSystem {
    public static void main(String[] args) {
        // Membuat Item
        Item item1 = new Item(2.5f, "Laptop");
        Item item2 = new Item(1.2f, "Mouse");

        // Membuat Customer
        Customer customer = new Customer("Erik", "123 Main Street");

        // Membuat OrderDetail
        OrderDetail detail1 = new OrderDetail(2, "Shipped", item1);
        OrderDetail detail2 = new OrderDetail(3, "Shipped", item2);

        // Membuat Order
        Order order = new Order(new Date(), "Processing");
        OrderDetail[] orderDetails = {detail1, detail2};

        // Menghitung total order
        float subTotal = order.calcSubTotal(orderDetails);
        float tax = order.calcTax(orderDetails);
        float total = order.calcTotal(orderDetails);
        float totalWeight = order.calcTotalWeight(orderDetails);

        // Format angka menjadi Rupiah
        @SuppressWarnings("deprecation")
        Locale indonesia = new Locale("id", "ID");
        NumberFormat rupiahFormat = NumberFormat.getCurrencyInstance(indonesia);

        // Output hasil perhitungan
        System.out.println("Customer Name: " + customer.getName());
        System.out.println("Order Subtotal: " + rupiahFormat.format(subTotal));
        System.out.println("Order Tax: " + rupiahFormat.format(tax));
        System.out.println("Order Total: " + rupiahFormat.format(total));
        System.out.println("Order Total Weight: " + totalWeight + " kg");

        // Membayar dengan Cash
        Payment payment = new Cash(total, total);
        System.out.println("Payment Authorized: " + payment.authorized());
    }
}
```
# OutPut
```
Customer Name: Erik
Order Subtotal: Rp500.000,00
Order Tax: Rp50.000,00
Order Total: Rp550.000,00
Order Total Weight: 7.5 kg
Payment Authorized: true
```
# Penjelasan
Format Mata Uang Rupiah:

Digunakan NumberFormat dengan Locale("id", "ID") untuk memformat angka ke dalam bentuk Rupiah.
Contoh output: Rp100.000,00.
Contoh Harga dan Pajak:

Harga per unit item adalah Rp100.000.
Pajak sebesar 10% diterapkan pada subtotal.
Berat Total:

Berat unit diatur secara statis pada OrderDetail.calcWeight() sebagai contoh.
Metode Pembayaran:

Pembayaran menggunakan Cash dengan validasi pada jumlah uang yang disediakan (cashTendered).

# ScreenShot
![Cuplikan layar 2024-12-05 082814](https://github.com/user-attachments/assets/30ebe633-eab7-49aa-84b8-433dc9958147)
![Cuplikan layar 2024-12-05 082908](https://github.com/user-attachments/assets/5e05cc91-d207-438c-b015-857db80fea0e)
