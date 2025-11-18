# abcd
1

Containerization (win1011 , ubuntu,Debian ,fedora, mac 10.15+)
64bit virtualization
4gb ram 
10gb 
empty internet
download== run installer == restart == ddesktop == --version
docker pull nginx
docker images 
docker rmi nginx
docker build -t myapp .

docker run ubuntu
docker run -it ubuntu bash
docker run -d nginx
docker run -p 8080:80 nginx
docker ps
docker ps -a
docker start id
docker restart id
docker stop id
docker rm id

docker logs mynginx
docker inspect mynginx

docker system prune
docker image prune
docker container prune


2

mkdir java-calculator
cd java-calculator

Calculator.java
import java.util.Scanner;
public class Calculator {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        double a = sc.nextDouble();
        double b = sc.nextDouble();
        char op = sc.next().charAt(0);
        double result = 0;
        if (op == '+') result = a + b;
        else if (op == '-') result = a - b;
        else if (op == '*') result = a * b;
        else if (op == '/') result = (b != 0) ? a / b : 0;
        else System.out.println("Invalid Operator");
        System.out.println(result);
    }
}

Dockerfile
FROM openjdk:17-slim
WORKDIR /app
COPY Calculator.java .
RUN javac Calculator.java
CMD ["java", "Calculator"]
docker build -t java-calculator .
docker run -it java-calculator
3

mkdir py-guess
cd py-guess

guess.py
import random
number = random.randint(1, 10)
print("Guess a number between 1 and 10:")
user = int(input())
if user == number:
    print("Correct!")
else:
    print("Wrong! The number was", number)

Dockerfile

FROM python:3.10-slim
WORKDIR /app
COPY guess.py .
CMD ["python", "guess.py"]

docker build -t py-guess .
docker run -it py-guess











4

mkdir simple-webapp
cd simple-webapp
app.py
app = Flask(__name__)
users = {}

@app.route("/register", methods=["GET", "POST"])
def register():
    if request.method == "GET":
        return """
        <h2>Registration</h2>
        <form method='POST'>
            Username: <input name='user'><br>
            Password: <input name='pass' type='password'><br><br>
            <button type='submit'>Register</button>
        </form>
        """
    users[request.form["user"]] = request.form["pass"]
    return "Registered!"

@app.route("/login", methods=["GET", "POST"])
def login():
    if request.method == "GET":
        return """
        <h2>Login</h2>
        <form method='POST'>
            Username: <input name='user'><br>
            Password: <input name='pass' type='password'><br><br>
            <button type='submit'>Login</button>
        </form>
        """
    u = request.form["user"]
    p = request.form["pass"]
    return "Login Successful" if u in users and users[u] == p else "Invalid"

@app.route("/")
def home():
    return "Simple Web App running. Use /register or /login"

app.run(host="0.0.0.0", port=5000)
Dockerfile
FROM python:3.10-slim
WORKDIR /app
COPY app.py .
RUN pip install flask
EXPOSE 5000
CMD ["python3", "app.py"]

docker build -t simple-webapp .
docker run -d -p 5000:5000 simple-webapp

docker logs cn 
http://localhost:5000/
register and login

6

to automate the deployment and execution

docker tag simple-webapp kdeepthi6002/simple-webapp:1.0
docker push kdeepthi6002/simple-webapp:1.0

service.yaml
apiVersion: v1
kind: Service
metadata:
  name: webapp-service
spec:
  type: NodePort
  selector:
    app: simple-webapp
  ports:
  - port: 5000
    targetPort: 5000
    nodePort: 30080

deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: simple-webapp
spec:
  replicas: 2
  selector:
    matchLabels:
      app: simple-webapp
  template:
    metadata:
      labels:
        app: simple-webapp
    spec:
      containers:
      - name: simple-webapp
        image: kdeepthi6002/simple-webapp:1.0
        ports:
        - containerPort: 5000

kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

kubectl get pods
kubectl get svc

http://localhost:30080








5

Ddesktop
Wsl –install
Settings == Kubernetes == enable 
Apply and restart

kubectl version --client
kubectl get nodes
kubectl cluster-info

kubectl get pods 
kubectl describe pod pn
kubectl delete pod pn
kubectl logs pn

kubectl create deployment webapp --image=nginx
kubectl get deployments
kubectl delete deployment webapp
kubectl get svc
kubectl describe svc name
kubectl delete svc webapp
kubectl get endpoints


kubectl create deployment demo --image=nginx
kubectl expose deployment demo --type=NodePort --port=80
kubectl get pods
kubectl get svc



7

Jdk
Ide
Selenium java client library  https://www.selenium.dev/downloads/
chrome driver https://googlechromelabs.github.io/chrome-for-testing/

1.Open NetBeans → New Project → Java Application
2.Add Selenium JAR Files to NetBeans Project
3.Write a Simple Automation Test in Java replace cdriver path

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class SeleniumTest {
    public static void main(String[] args) throws Exception {

        System.setProperty("webdriver.chrome.driver",
            "C:\\SeleniumDrivers\\chromedriver.exe");

        WebDriver driver = new ChromeDriver();

        driver.get("https://www.google.com");

        driver.findElement(By.name("q")).sendKeys("Selenium Automation Testing");

        driver.findElement(By.name("q")).submit();

        System.out.println("Page Title: " + driver.getTitle());

        Thread.sleep(3000);  // Keep browser open for 3 seconds
        driver.quit();
    }
}

Run 
Page Title: Selenium Automation Testing - Google Search

8

Create a Simple JavaScript Web Application
<!DOCTYPE html>
<html>
<body>

<h2>JavaScript Demo</h2>

<p id="msg">Original Text</p>

<button id="btn" onclick="changeText()">Click Me</button>

<script>
function changeText() {
    document.getElementById("msg").innerText = "Button Clicked Using JavaScript!";
}
</script>

</body>
</html>

TestJSApp.java inside your project.

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class TestJSApp {

    public static void main(String[] args) throws Exception {

        // Set ChromeDriver path
        System.setProperty("webdriver.chrome.driver",
                "C:\\SeleniumDrivers\\chromedriver.exe");

        WebDriver driver = new ChromeDriver();

        // Load the JavaScript web page
        driver.get("file:///C:/Users/K%20RAJU/Downloads/testpage.html");

        // Click button
        driver.findElement(By.id("btn")).click();

        // Read output text
        String text = driver.findElement(By.id("msg")).getText();
        System.out.println("Automation Output: " + text);

        Thread.sleep(3000); // to view result
        driver.quit();
    }
}
Automation Output: Button Clicked Using JavaScript!


9

STEP 1 — Create the Calculator Application (JavaScript + HTML)

<!DOCTYPE html>
<html>
<head>
    <title>Simple Calculator</title>
</head>
<body>

<h2>Simple Calculator</h2>

<input id="num1" type="number" placeholder="Enter number 1"><br><br>
<input id="num2" type="number" placeholder="Enter number 2"><br><br>

<select id="op">
    <option value="+">+</option>
    <option value="-">-</option>
    <option value="*">*</option>
    <option value="/">/</option>
</select>
<br><br>

<button id="calcBtn" onclick="calculate()">Calculate</button>

<h3 id="result"></h3>

<script>
function calculate() {
    let a = parseFloat(document.getElementById("num1").value);
    let b = parseFloat(document.getElementById("num2").value);
    let op = document.getElementById("op").value;

    if (isNaN(a) || isNaN(b)) {
        document.getElementById("result").innerText = "Please enter valid numbers!";
        return;
    }

    let res = 0;

    if (op === "+") res = a + b;
    else if (op === "-") res = a - b;
    else if (op === "*") res = a * b;
    else if (op === "/") res = b !== 0 ? a / b : "Division by zero!";

    document.getElementById("result").innerText = "Result: " + res;
}
</script>

</body>
</html>

TestCalculator.java

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.Select;

public class TestCalculator {

    public static void main(String[] args) throws Exception {

        System.setProperty("webdriver.chrome.driver",
                "C:\\SeleniumDrivers\\chromedriver.exe");

        WebDriver driver = new ChromeDriver();

        // Load calculator file
        driver.get("file:///C:/Users/K%20RAJU/Downloads/calculator.html");

        // TC01: Addition
        driver.findElement(By.id("num1")).sendKeys("10");
        driver.findElement(By.id("num2")).sendKeys("20");
        new Select(driver.findElement(By.id("op"))).selectByValue("+");
        driver.findElement(By.id("calcBtn")).click();
        System.out.println("TC01: " + driver.findElement(By.id("result")).getText());

        Thread.sleep(1000);

        // TC02: Subtraction
        driver.navigate().refresh();
        driver.findElement(By.id("num1")).sendKeys("50");
        driver.findElement(By.id("num2")).sendKeys("30");
        new Select(driver.findElement(By.id("op"))).selectByValue("-");
        driver.findElement(By.id("calcBtn")).click();
        System.out.println("TC02: " + driver.findElement(By.id("result")).getText());

        Thread.sleep(1000);

        // TC03: Multiplication
        driver.navigate().refresh();
        driver.findElement(By.id("num1")).sendKeys("6");
        driver.findElement(By.id("num2")).sendKeys("7");
        new Select(driver.findElement(By.id("op"))).selectByValue("*");
        driver.findElement(By.id("calcBtn")).click();
        System.out.println("TC03: " + driver.findElement(By.id("result")).getText());

        Thread.sleep(1000);

        // TC04: Division
        driver.navigate().refresh();
        driver.findElement(By.id("num1")).sendKeys("40");
        driver.findElement(By.id("num2")).sendKeys("8");
        new Select(driver.findElement(By.id("op"))).selectByValue("/");
        driver.findElement(By.id("calcBtn")).click();
        System.out.println("TC04: " + driver.findElement(By.id("result")).getText());

        Thread.sleep(2000);

        driver.quit();
    }
}
10

Create the Registration HTML File

<!DOCTYPE html>
<html>
<body>

<h2>Simple Registration Form</h2>

<label>Name:</label><br>
<input id="name" type="text"><br><br>

<label>Email:</label><br>
<input id="email" type="email"><br><br>

<label>Password:</label><br>
<input id="password" type="password"><br><br>

<button id="registerBtn" onclick="register()">Register</button>

<h3 id="msg"></h3>

<script>
function register() {
    let name = document.getElementById("name").value;
    let email = document.getElementById("email").value;
    let pass = document.getElementById("password").value;

    if (name === "" || email === "" || pass === "") {
        document.getElementById("msg").innerText = "All fields are required!";
    } else {
        document.getElementById("msg").innerText = "Registration Successful!";
    }
}
</script>

</body>
</html>








import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class TestRegistration {

    public static void main(String[] args) throws Exception {

        System.setProperty("webdriver.chrome.driver",
                "C:\\Users\\K RAJU\\Downloads\\chromedriver-win64\\chromedriver-win64\\chromedriver.exe");

        WebDriver driver = new ChromeDriver();

        // Load the HTML file
        driver.get("file:///C:/Users/K%20RAJU/Downloads/index.html");

        // TC01: Valid Registration
        driver.findElement(By.id("name")).sendKeys("John");
        driver.findElement(By.id("email")).sendKeys("john@gmail.com");
        driver.findElement(By.id("password")).sendKeys("12345");
        driver.findElement(By.id("registerBtn")).click();
        System.out.println("TC01: " + driver.findElement(By.id("msg")).getText());

        // TC02: Missing Name
        driver.navigate().refresh();
        driver.findElement(By.id("email")).sendKeys("john@gmail.com");
        driver.findElement(By.id("password")).sendKeys("12345");
        driver.findElement(By.id("registerBtn")).click();
        System.out.println("TC02: " + driver.findElement(By.id("msg")).getText());

        // TC03: Missing Email
        driver.navigate().refresh();
        driver.findElement(By.id("name")).sendKeys("John");
        driver.findElement(By.id("password")).sendKeys("12345");
        driver.findElement(By.id("registerBtn")).click();
        System.out.println("TC03: " + driver.findElement(By.id("msg")).getText());

        // TC04: Missing Password
        driver.navigate().refresh();
        driver.findElement(By.id("name")).sendKeys("John");
        driver.findElement(By.id("email")).sendKeys("john@gmail.com");
        driver.findElement(By.id("registerBtn")).click();
        System.out.println("TC04: " + driver.findElement(By.id("msg")).getText());

        // TC05: All Fields Empty
        driver.navigate().refresh();
        driver.findElement(By.id("registerBtn")).click();
        System.out.println("TC05: " + driver.findElement(By.id("msg")).getText());

        // Keep browser open for 3 seconds
        Thread.sleep(3000);

        driver.quit();
    }
}
