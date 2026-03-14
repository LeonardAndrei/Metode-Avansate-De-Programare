sequenceDiagram

    participant C as Client
    participant OS as OrderService
    participant Repo as IOrderRepository

    C->>OS: CancelOrder(orderId)
    
    %% Cautam comanda in baza de date
    OS->>Repo: FindById(orderId)
    Repo-->>OS: order
    
    %% Verificam statusul comenzii
    alt order.Status == "Expediata"
        %% Nu mai putem anula
        OS-->>C: throw InvalidOperationException("Comanda e deja pe drum!")
    else order.Status != "Expediata"
        %% Putem anula
        OS->>OS: order.Cancel()
        
        %% Salvam noul status in baza de date
        OS->>Repo: Save(order)
        Repo-->>OS: OK
        
        OS-->>C: 200 OK (Comanda anulata)
    end
