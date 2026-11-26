# InsurTech-Claim-Processor
# InsurTech: Automated Claim Processor

A console-based backend application, written in Python, that helps an insurance company manage customer claims, calculate policy payouts and handle appeals. It was developed as the final project for **CS-2012 Object-Oriented Programming** in the BS Financial Technology program at FAST-NUCES, Karachi.

## Overview

When customers file insurance claims, someone has to decide who is processed first, how much each claim should pay, and what happens when a claim is denied. InsurTech models that workflow in code. Claims enter a waiting line, are reviewed in the order they arrive, and are approved or denied by an administrator. Denied claims can be appealed, and the most recent appeal is always reviewed first. Approved claims are saved to disk, and a financial report can be exported at any time.

The project has two aims. The first is to apply the core principles of object-oriented programming to a realistic financial scenario. The second is to build the supporting data structures and algorithms by hand, rather than relying on Python's built-in shortcuts.

## Features

- Submit new claims of three types: Health, Auto and Property
- Automatic payout calculation based on claim type
- Validation of claim amounts and policy status, with clear error messages
- First-come, first-served processing of pending claims
- Appeal review that handles the most recent denial first
- Ranking of pending claims by payout, from highest to lowest
- Fast lookup of any claim by its unique ID
- Saving of approved claims to JSON and export of a full report to CSV

## Object-Oriented Design

| Principle | How it is used in InsurTech |
|---|---|
| Abstraction | `Claim` is an abstract base class that declares `calculate_payout()` without implementing it. A generic `Claim` cannot be created directly. |
| Inheritance | `HealthClaim`, `AutoClaim` and `PropertyClaim` inherit shared attributes and methods from `Claim` and supply their own payout rules. |
| Polymorphism | The sorting and reporting code calls `calculate_payout()` on any claim without checking its type. Each subclass responds with its own rule. |
| Encapsulation | Claim ID, customer name, amount and status are private attributes. They are reached only through getters and validated setters. |
| Exception handling | `InvalidClaimAmountError` and `PolicyExpiredException` stop invalid data at the point of creation, so the program continues running safely. |

### Payout Rules

Each claim type applies a flat deductible and then covers a percentage of what remains. If a claim is smaller than its deductible, the payout is zero.

| Claim type | Deductible | Coverage |
|---|---|---|
| Health | $200 | 80% |
| Auto | $500 | 90% |
| Property | $1,000 | 85% |

For example, a Health claim of $2,200 pays (2,200 − 200) × 0.80 = **$1,600**.

## Data Structures and Algorithms

| Component | Structure or algorithm | Reason for the choice |
|---|---|---|
| Pending claims | Queue (FIFO) | Claims are processed strictly in the order they were submitted, which keeps the process fair. |
| Appeals | Stack (LIFO) | The most recently denied claim is reviewed first, so recent complaints are handled promptly. |
| Claim history | Singly linked list | History is append-only, so the list can grow without resizing or shifting elements. |
| Ranking | Bubble sort | Orders pending claims by payout, highest first. It sorts a copy, so the queue order is never disturbed. |
| Search | Binary search | Finds a claim by ID in logarithmic time once the claims are ordered by ID. |
| Persistence | JSON and CSV | JSON stores approved claims in a structured form. CSV produces a report that opens easily in a spreadsheet. |

## Project Structure

```
InsurTech_Project/
├── data/
│   ├── claims.json            # Approved claims saved by the program
│   └── reports.csv            # Exported claim report
├── models/
│   ├── claims.py              # Claim base class and its subclasses
│   └── exceptions.py          # Custom exception classes
├── core/
│   ├── data_structures.py     # Linked list, queue and stack
│   └── processing_engine.py   # Sorting, searching and file handling
└── main.py                    # Entry point and menu interface
```

## Getting Started

### Requirements

- Python 3.8 or later
- No external libraries are needed, since the project uses only the Python standard library

### Running the Program

1. Clone the repository:
   ```
   git clone https://github.com/Iman12-umm/InsurTech-Claim-Processor.git
   ```
2. Move into the project folder:
   ```
   cd InsurTech-Claim-Processor
   ```
3. Start the application:
   ```
   python main.py
   ```

Please run the program from the project's root folder, since the data files are located using relative paths.

## Using the Application

The program opens with a numbered menu:

| Option | Action |
|---|---|
| 1 | Submit a new claim. The claim is added to the pending queue and the history list. |
| 2 | Process the next pending claim. The administrator approves or denies it. Denied claims move to the appeals stack. |
| 3 | Review the latest appeal. The administrator may overturn the denial or uphold it. |
| 4 | Rank all pending claims by payout using bubble sort. |
| 5 | Search the claim history by claim ID using binary search. |
| 6 | Save approved claims to `data/claims.json` and export the full report to `data/reports.csv`. |
| 7 | Exit the program. |

A typical session might look like this: submit a few claims with option 1, rank them with option 4, process them with option 2, review any denied claims with option 3, and finish by saving with option 6.

## Team and Contributions

| Member | Roll number | Responsibilities |
|---|---|---|
| Suhaina Ilyas Khan | 25K-5743 | Abstract base classes and polymorphism, custom linked list, binary search, JSON and CSV file handling |
| Umm e Imaan | 25K-5611 | Stack and queue implementations, bubble sort, custom exception handling |
| Ashka | 25K-5608 | Basic class attributes, user menu interface, final report document |

## Limitations and Future Work

This project was built to demonstrate core concepts, and there is room to grow. Some improvements we would consider next:

- Reload previously saved claims from `claims.json` when the program starts
- Reject duplicate claim IDs at submission
- Use a double-ended queue for faster removal from the front, and add a tail pointer to the linked list so that appending is constant time
- Add an early-exit check to bubble sort so that already-sorted input finishes quickly
- Keep a separate history for each customer
- Replace the console menu with a graphical interface or connect the system to a database

## Acknowledgements

This project was completed under the guidance of our course instructor for CS-2012, Object-Oriented Programming, at FAST-NUCES, Karachi.
