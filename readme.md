# dmoney JMeter Performance Test

## Project Overview

This project contains a JMeter performance test suite for the **dmoney** financial transaction API. The test simulates real-world usage scenarios involving Agents, Customers, and Merchants performing various financial transactions concurrently.

---

## Technology Used

- **Apache JMeter 5.6.3** — Performance testing tool
- **dmoney REST API** — Backend under test
- **CSV Data Files** — Test data management
- **HTML Report** — Result visualization

---

## Prerequisites

- Java JDK 8 or higher
- Apache JMeter 5.6.3 or higher
- dmoney backend running on `http://localhost:5000`
- dmoney frontend running on `http://localhost:3000`

---

## How to Run

### Run in GUI Mode (for debugging)

```bash
jmeter -t dmoney.jmx
```

### Run in CLI Mode (for report generation)

```bash
jmeter -n -t dmoney.jmx -l result.jtl -e -o Reports
```

### Open the HTML Report

After execution, open the following file in your browser:

```bash
Reports/index.html
```

---

# Test Scenarios

## Thread Group 1 — Deposit (Agent → Customer)

| Setting           | Value       |
| ----------------- | ----------- |
| Number of Threads | 5           |
| Ramp-up Period    | 120 seconds |
| Loop Count        | 2           |
| Total Requests    | 10          |

### Scenario Details

- 5 Agents each deposit money to 2 Customers
- Total of 10 deposit transactions executed
- Amount is randomized between **10 BDT to 100 BDT**

---

## Thread Group 2 — Send Money (Customer → Customer)

| Setting           | Value       |
| ----------------- | ----------- |
| Number of Threads | 5           |
| Ramp-up Period    | 120 seconds |
| Loop Count        | 2           |
| Total Requests    | 10          |

### Scenario Details

- 5 Customers each send money to 2 other Customers
- Total of 10 send money transactions executed
- Amount is randomized between **10 BDT to 100 BDT**

---

## Thread Group 3 — Payment (Customer → Merchant)

| Setting           | Value       |
| ----------------- | ----------- |
| Number of Threads | 5           |
| Ramp-up Period    | 120 seconds |
| Loop Count        | 1           |
| Total Requests    | 5           |

### Scenario Details

- 5 Customers each make payments to Merchants
- Total of 5 payment transactions executed
- Amount is randomized between **10 BDT to 100 BDT**

---

# Test Data

## CSV Files

| File Name       | Purpose                       | Columns                                     |
| --------------- | ----------------------------- | ------------------------------------------- |
| `deposit.csv`   | Agent → Customer deposits     | `agentToken`, `fromAccount`, `toAccount`    |
| `sendMoney.csv` | Customer → Customer transfers | `customerToken`, `fromAccount`, `toAccount` |
| `payment.csv`   | Customer → Merchant payments  | `customerToken`, `fromAccount`, `toAccount` |

---

## Accounts Used

| Role     | Count | Phone Number Range        |
| -------- | ----- | ------------------------- |
| Agent    | 5     | 01700000001 – 01700000005 |
| Customer | 15    | 01700000006 – 01700000020 |
| Merchant | 2     | 01700000021 – 01700000022 |

---

# JMeter Test Plan Structure

```text
Test Plan
├── HTTP Header Manager
│   ├── Content-Type: application/json
│   └── X-AUTH-SECRET-KEY: ROADTOCAREER
│
├── Thread Group 1 — Deposit
│   ├── CSV Data Set Config (deposit.csv)
│   ├── Random Variable (amount: 10–100)
│   ├── HTTP Header Manager (Authorization)
│   ├── HTTP Request — POST /transaction/deposit
│   │   ├── Response Assertion (200/201)
│   │   └── JSON Assertion ($.message)
│
├── Thread Group 2 — Send Money
│   ├── CSV Data Set Config (sendMoney.csv)
│   ├── Random Variable (amount: 10–100)
│   ├── HTTP Header Manager (Authorization)
│   ├── HTTP Request — POST /transaction/sendmoney
│   │   ├── Response Assertion (200/201)
│   │   └── JSON Assertion ($.message)
│
├── Thread Group 3 — Payment
│   ├── CSV Data Set Config (payment.csv)
│   ├── Random Variable (amount: 10–100)
│   ├── HTTP Header Manager (Authorization)
│   ├── HTTP Request — POST /transaction/payment
│   │   ├── Response Assertion (200/201)
│   │   └── JSON Assertion ($.message)
│
└── View Results Tree
```

---

# API Endpoints Tested

| Transaction Type | Method | Endpoint                 | Authentication |
| ---------------- | ------ | ------------------------ | -------------- |
| Deposit          | POST   | `/transaction/deposit`   | Agent Token    |
| Send Money       | POST   | `/transaction/sendmoney` | Customer Token |
| Payment          | POST   | `/transaction/payment`   | Customer Token |

---

## Test Results
### Result image: 
<img width="1898" height="1375" alt="result" src="https://github.com/user-attachments/assets/b7611cb1-0045-4b46-b12d-4e9869d4f3a2" />

---

## Request Headers

```http
Content-Type: application/json
X-AUTH-SECRET-KEY: ROADTOSDET
Authorization: Bearer <token>
```

---

# Performance Highlights

- ✅ **0% Error Rate** across all executed transactions
- ✅ **APDEX Score: 1.000** (Perfect) for all transaction types
- ✅ **100% Pass Rate** in Request Summary
- ✅ Average response time remained under **100 ms**
- ✅ All assertions passed successfully

---

# Assertions Used

| Assertion Type     | Purpose                                                    |
| ------------------ | ---------------------------------------------------------- |
| Response Assertion | Verifies HTTP status code is `200` or `201`                |
| JSON Assertion     | Verifies `$.message` contains the expected success message |

---

# Author

- **Name:** Asadulla Al Mamun
- **Topic:** Performance Testing with JMeter using dmoney API
