# 💳 Kahf Gold-Backed Card System

This project outlines the flows used in Kahf’s gold-backed debit card system.

---

## 🪙 Flow 1: User Deposits Money and Acquires Gold

```mermaid
flowchart TD
    subgraph UK [UK]
        user((User)):::actor
        ukAccount[/"Kahf UK Bank Account"/]:::bank
    end

    subgraph Turkey [Turkey]
        goldBank[/"Gold Bank (Turkey)"/]:::bank
    end

    subgraph KahfSystem [Kahf Internal System]
        allocation[[Allocate gold to user's balance]]
        ledger[(Update global gold ledger)]
        notify[Notify user of gold ownership]
    end

    user -->|Deposit GBP| ukAccount
    ukAccount -->|Transfer funds| goldBank
    goldBank -->|Buy and store gold| allocation
    allocation --> ledger
    ledger --> notify
    notify -->|Confirmation| user

    classDef actor fill:#f9f,stroke:#333,stroke-width:2px;
    classDef bank fill:#bbf,stroke:#333,stroke-width:1.5px;
```

## Flow 2: User performs a card transaction
```mermaid

graph TD
    %% Define the entities/actors
    subgraph Entities
        A[User]
        B[External Card Partner]
        C[Kahf]
        D[Global Gold Ledger]
        E[Turkey's Gold Bank]
        F[Receiving Merchant]
    end

    %% Flow 2: User Makes a Payment Using Their Debit Card
    subgraph Flow 2: User Makes a Payment
        1[User Makes Payment via Card] --> |1. Debit Card Transaction| 2{External Card Partner Authorizes & Settles}
        2 --> |1a. Cash Paid from Card Partner's Pool| F
        2 --> |2. Real-time API Notification of Payment| C
        C --> |2a. Calculate Gold Equivalent & Update User's Gold Balance| D
        D --> |2b. User's Gold Balance Reduced| C

        subgraph Overnight Reconciliation & Replenishment
            3[Overnight: Card Partner Tallies All Payments] --> |3a. Reconciliation Report| C
            C --> |4. Instruct Gold Bank to Liquidate Gold from Kahf's overall balance| E
            E --> |4a. Cash Out Gold e.g., to USD/GBP| G[Liquidated Cash]
            G --> |5. Transfer Cash to Card Partner's Liquidity Account| B
            B --> |5a. Replenish Card Partner's Cash Pool| B_End(Card Partner's Cash Pool Ready)
        end

        B_End --> |5b. Ready for Future Payments| 1
    end

    %% Link the main entities for overall context
    C -- "Manages" --> D
    E -- "Holds & Liquidates Gold" --> C
    B -- "Issues Card & Manages Cash" --> A
```
