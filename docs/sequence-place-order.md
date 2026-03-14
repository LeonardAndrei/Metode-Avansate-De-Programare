sequenceDiagram
    participant C as Client
    participant OS as OrderService
    participant PP as IPaymentProcessor
    participant Repo as IOrderRepository
    participant Email as IEmailService

    C->>OS: PlaceOrder(order)
    
    %% Verificam plata mai intai
    OS->>PP: Process(order.Amount)
    
    alt Plata acceptata
        PP-->>OS: Success
        
        %% Daca plata e ok, salvam in baza de date
        OS->>Repo: Save(order)
        Repo-->>OS: OrderId
        
        %% Trimitem email
        OS->>Email: SendConfirmation(order)
        Email-->>OS: OK
        
        %% Returnam succes clientului
        OS-->>C: 200 OK (Order Created)
    else Plata respinsa
        PP-->>OS: Declined
        OS-->>C: 400 Bad Request (Payment Failed)
    end