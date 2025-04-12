# Atlas SQL

## Prepare Atlas cluster and collections to use for Atlas SQL testing
![image](https://github.com/user-attachments/assets/39cab03a-fb7c-48f7-86cc-20fd3d77d69a)

## Configure Atlas SQL
Click Connect
![image](https://github.com/user-attachments/assets/f645a0b9-aa36-4649-8553-b09d5a0e9fcf)

Choose Atlas SQL
![image](https://github.com/user-attachments/assets/82df66f1-79d7-4e7c-959d-34e79f5e1a8f)

If you have not configured any Atlas Data Federation instance, you will be asked to create one, and it will take around 7-10 minutes to get Atlas do it for you.

Once Data Federation instance is created, you will see this screen where you can choose how you would like to connect to it and which database. Then copy the JDBC URL and Port number.

![image](https://github.com/user-attachments/assets/c8233a86-8794-4656-82b8-3d4e5c591497)

To further configure the SQL Schemas, you can do it here ![image](https://github.com/user-attachments/assets/78acc26c-1002-4f59-9d61-f408e7f8858d)

## Try connecting from simple Java MVN

```
package com.fktech;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.Properties;

/**
 * Hello world!
 */
public class App {
    public static void main(String[] args) {
        System.out.println("Retrieving data from Atlas SQL");

        try {

            // Loading and registering drivers
            // Optional from JDBC version 4.0
            Class.forName("com.mongodb.jdbc.MongoDriver");

            // Step 2:Establishing a connection
            Connection con = DriverManager.getConnection(
                    "jdbc:mongodb://atlas-sql-67aaa1cbe17121575cc2e424-hixjx.a.query.mongodb.net:27017/sample_analytics?ssl=true&authSource=admin",
                    "xxxxx", "xxxxxx");

            // Step 3: Creating statement
            Statement st = con.createStatement();

            // Step 4: Executing the query and storing the
            // result
            ResultSet rs = st.executeQuery(
                    "SELECT * FROM transactions limit 1");

            // Step 5: Processing the results
            while (rs.next()) {
                System.out.println("ID: " + rs.getInt("account_id") + ", Transaction counts: " +
                        rs.getInt("transaction_count") + ", Start Dt: " + rs.getDate("bucket_start_date"));
            }

            // Step 6: Closing the connections
            // using close() method to release memory resources
            con.close();

            // Display message for successful execution of program
            System.out.println("Steps in Setting Up of JDBC");
        } catch (Exception ex) {
            ex.printStackTrace();
        }

    }
}
```

Make sure you add this dependency into your POM.XML
```
    <dependency>
      <groupId>org.mongodb</groupId>
      <artifactId>mongodb-jdbc</artifactId>
      <version>2.2.3</version>
    </dependency>
```

Run the program
```
mvn clean compile exec:java -Dexec.mainClass="com.fktech.App"
```

Output, as you can then see the record is printed in the console
```
Retrieving data from Atlas SQL
SLF4J: No SLF4J providers were found.
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See https://www.slf4j.org/codes.html#noProviders for further details.
ID: 443178, Transaction counts: 66, Start Dt: 1969-02-04
Steps in Setting Up of JDBC
```


