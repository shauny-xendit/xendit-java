# Xendit Java Library

## Table of Contents

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [API Documentation](#api-documentation)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
  - [Disbursement Services](#disbursement-services)
    - [Create a disbursement](#create-a-disbursement)
    - [Get banks with available disbursement service](#get-banks-with-available-disbursement-service)
    - [Get a disbursement by external ID](#get-a-disbursement-by-external-id)
    - [Get a disbursement by ID](#get-a-disbursement-by-id)
  - [Invoice services](#invoice-services)
    - [Create an invoice](#create-an-invoice)
    - [Get an invoice by ID](#get-an-invoice-by-id)
    - [Get all invoices](#get-all-invoices)
    - [Expire an invoice](#expire-an-invoice)
  - [Virtual Account Services](#virtual-account-services)
    - [Create a fixed virtual account](#create-a-fixed-virtual-account)
      - [Closed virtual account](#closed-virtual-account)
      - [Opened virtual account](#opened-virtual-account)
    - [Get banks with available virtual account service](#get-banks-with-available-virtual-account-service)
    - [Get a fixed virtual account by ID](#get-a-fixed-virtual-account-by-id)
    - [Get a fixed virtual account payment by payment ID](#get-a-fixed-virtual-account-payment-by-payment-id)
- [Contributing](#contributing)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## API Documentation
Please check [Xendit API Reference](https://xendit.github.io/apireference/).

## Requirements
JDK 1.7 or later.

## Installation
Maven:
```
<dependency>
  <groupId>com.xendit</groupId>
  <artifactId>xendit-java-lib</artifactId>
  <version>1.3.0</version>
  <type>pom</type>
</dependency>
```

Gradle:
```
compile 'com.xendit:xendit-java-lib:1.3.0'
```

More information: https://bintray.com/xendit/android/xendit-java-lib

## Usage
You need to use secret API key in order to use functionality in this library. The key can be obtained from your [Xendit Dasboard](https://dashboard.xendit.co/settings/developers#api-keys).

```java
import com.xendit.Xendit;

public class Example {
    public static void main(String[] args) {
        Xendit.apiKey = "PUT YOUR API KEY HERE";
    }
}
```

There are some examples provided for you [here](https://github.com/xendit/xendit-java-library/tree/master/xendit-java-library-example/src/main/java).

### Disbursement Services

Example: Create a disbursement

```java
import com.xendit.Xendit;
import com.xendit.exception.XenditException;
import com.xendit.model.Disbursement;

import java.util.HashMap;
import java.util.Map;

public class ExampleCreateDisbursement {
    public static void main(String[] args) {
        Xendit.apiKey = "xnd_development_...";

        try {
            Map<String, Object> params = new HashMap<>();
            params.put("external_id", "my_external_id");
            params.put("bank_code", "BCA");
            params.put("account_holder_name", "John Doe");
            params.put("account_number", "123456789");
            params.put("description", "My Description");
            params.put("amount", "90000");

            Disbursement disbursement = Disbursement.create(params);
        } catch (XenditException e) {
            e.printStackTrace();
        }
    }
}
```

#### Create a disbursement

You can choose whether want to put the attributes as parameters or to put in inside a Map object.

```java
Disbursement.create(
    String externalId,
    String bankCode,
    String accountHolderName,
    String accountNumber,
    String description,
    BigInteger amount,
    String[] emailTo,
    String[] emailCc,
    String[] emailBcc
);
```

```java
Disbursement.create(
    Map<String, Object> params
);
```

#### Get banks with available disbursement service

```java
AvailableBank[] banks = Disbursement.getAvailableBanks();
```

#### Get a disbursement by external ID

```java
Disbursement disbursement = Disbursement.getByExternalId("EXAMPLE_ID");
```

#### Get a disbursement by ID

```java
Disbursement disbursement = Disbursement.getById("EXAMPLE_ID");
```

### Invoice services

Example: Create an invoice

```java
import com.xendit.Xendit;
import com.xendit.exception.XenditException;
import com.xendit.model.Invoice;

import java.util.HashMap;
import java.util.Map;

public class ExampleCreateInvoice {
    public static void main(String[] args) {
        Xendit.apiKey = "xnd_development_...";

        try {
            Map<String, Object> params = new HashMap<>();
            params.put("external_id", "my_external_id");
            params.put("amount", 1800000);
            params.put("payer_email", "customer@domain.com");
            params.put("description", "Invoice Demo #123");

            Invoice invoice = Invoice.create(params);
        } catch (XenditException e) {
            e.printStackTrace();
        }
    }
}

```

#### Create an invoice

You can choose whether want to put the attributes as parameters or to put in inside a Map object.

```java
Invoice.create(
    String externalId,
    Number amount,
    String payerEmail,
    String description
);
```

```java
Invoice.create(
    Map<String, Object> params
);
```

#### Get an invoice by ID

```java
Invoice invoice = Invoice.getById("EXAMPLE_ID");
```

#### Get all invoices

```java
Map<String, Object> params = new HashMap<>();
params.put("limit", 3);
params.put("statuses", "[\"SETTLED\",\"EXPIRED\"]");

Invoice[] invoices = Invoice.getAll(params);
```

#### Expire an invoice

```java
Invoice invoice = Invoice.expire("EXAMPLE_ID");
```

### Virtual Account Services

Example: Create a opened fixed virtual account

```java
import com.xendit.Xendit;
import com.xendit.enums.BankCode;
import com.xendit.exception.XenditException;
import com.xendit.model.FixedVirtualAccount;

import java.util.HashMap;
import java.util.Map;

public class ExampleCreateOpenVA {
    public static void main(String[] args) {
        Xendit.apiKey = "xnd_development_...";

        try {
            Map<String, Object> openVAMap = new HashMap<>();
            openVAMap.put("external_id", "my_external_id");
            openVAMap.put("bank_code", BankCode.BNI.getText());
            openVAMap.put("name", "John Doe");

            FixedVirtualAccount virtualAccount = FixedVirtualAccount.createOpen(openVAMap);
        } catch (XenditException e) {
            e.printStackTrace();
        }
    }
}

```

#### Create a fixed virtual account

You can choose whether want to put the attributes as parameters or to put in inside a Map object.

##### Closed virtual account

```java
FixedVirtualAccount.createClosed(
    String externalId,
    String bankCode,
    String name,
    Long expectedAmount,
    Map<String, Object> additionalParam
);
```

```java
FixedVirtualAccount.createClosed(
    Map<String, Object> params
);
```

##### Opened virtual account

```java
FixedVirtualAccount.createOpen(
    String externalId,
    String bankCode,
    String name,
    Map<String, Object> additionalParam
);
```

```java
FixedVirtualAccount.createOpen(
    Map<String, Object> params
);
```

#### Get banks with available virtual account service

```java
AvailableBank[] availableBanks = FixedVirtualAccount.getAvailableBanks();
```

#### Get a fixed virtual account by ID

```java
FixedVirtualAccount fpa = FixedVirtualAccount.getFixedVA("EXAMPLE_ID");
```

#### Get a fixed virtual account payment by payment ID

```java
FixedVirtualAccountPayment payment = FixedVirtualAccount.getPayment("EXAMPLE_PAYMENT_ID");
```

## Contributing

Running test suite

```
./gradlew test
```

For any requests, bugs, or comments, please [open an issue](https://github.com/xendit/xendit-java-library/issues) or [submit a pull request](https://github.com/xendit/xendit-java-library/pulls).