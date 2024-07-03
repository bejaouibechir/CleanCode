# Catalogue de Refactorings en C# (.NET 8)

## 1. **Changement de signature de fonction** (Change Function Declaration)
Modifie les paramètres d'une fonction.

***Exemple :***
```CSharp
// Avant
public void PrintMessage(string message) { ... }

// Après
public void PrintMessage(string message, bool isUrgent) { ... }
```

## 2. **Ajout de paramètre** (Add Parameter)
Ajoute un paramètre à une fonction existante.

***Exemple :***
```CSharp
// Avant
public void CalculateTotal(int quantity) { ... }

// Après
public void CalculateTotal(int quantity, decimal price) { ... }
```

## 3. **Suppression de paramètre** (Remove Parameter)
Supprime un paramètre d'une fonction.

***Exemple :***
```CSharp
// Avant
public void ProcessOrder(int orderId, string customerName) { ... }

// Après
public void ProcessOrder(int orderId) { ... }
```

## 4. **Renommage de fonction** (Rename Function)
Change le nom d'une fonction.

***Exemple :***
```CSharp
// Avant
public void SaveData(string data) { ... }

// Après
public void PersistData(string data) { ... }
```

## 5. **Renommage de méthode** (Rename Method)
Change le nom d'une méthode dans une classe.

***Exemple :***
```CSharp
// Avant
public void CalculateDiscount() { ... }

// Après
public void ApplyDiscount() { ... }
```

## 6. **Changement de référence en valeur** (Change Reference to Value)
Transforme une référence en une valeur.

***Exemple :***
```CSharp
// Avant
public class Currency { ... }

// Après
public struct Currency { ... }
```

## 7. **Changement de valeur en référence** (Change Value to Reference)
Transforme une valeur en une référence.

***Exemple :***
```CSharp
// Avant
public struct Point { ... }

// Après
public class Point { ... }
```

## 8. **Réduction de hiérarchie** (Collapse Hierarchy)
Fusionne des classes ou des fonctions.

***Exemple :***
```CSharp
// Avant
class Animal { ... }
class Mammal : Animal { ... }

// Après
class Animal { ... }
```

## 9. **Décomposition conditionnelle** (Decompose Conditional)
Divise une condition complexe en sous-conditions plus simples.

***Exemple :***
```CSharp
// Avant
if (isWeekend && isHoliday) { ... }

// Après
if (IsWeekend()) { ... }
if (IsHoliday()) { ... }
```

## 10. **Encapsulation de collection** (Encapsulate Collection)
Encapsule une collection (tableau, liste, etc.) dans une classe.

***Exemple :***
```CSharp
// Avant
string[] names = { "Alice", "Bob", "Charlie" };

// Après
class NamesCollection { ... }
```

<!-- Continue with other refactorings -->

## 11. **Encapsulation de variable** (Encapsulate Variable)
Encapsule une variable pour la rendre privée et fournir des méthodes d'accès.

***Exemple :***
```CSharp
// Avant
public class Order
{
    public string OrderId { get; set; }
    // ...
}

// Après
public class Order
{
    private string _orderId;

    public string GetOrderId() => _orderId;
    public void SetOrderId(string orderId) => _orderId = orderId;
    // ...
}
```

## 12. **Extraction de classe** (Extract Class)
Crée une nouvelle classe à partir d'une partie d'une classe existante.

***Exemple :***
```CSharp
// Avant
public class Customer
{
    public string Name { get; set; }
    public Address BillingAddress { get; set; }
    // ...
}

// Après
public class Customer
{
    public string Name { get; set; }
    public BillingInfo BillingInfo { get; set; }
    // ...
}

public class BillingInfo
{
    public Address BillingAddress { get; set; }
    // ...
}
```

## 13. **Extraction de fonction** (Extract Function)
Divise une partie de code en une nouvelle fonction.

***Exemple :***
```CSharp
// Avant
public void ProcessOrder(Order order)
{
    // ... code handling order details ...
    // ... more complex logic ...
}

// Après
public void ProcessOrder(Order order)
{
    ValidateOrder(order);
    CalculateTotal(order);
    // ... other steps ...
}

private void ValidateOrder(Order order)
{
    // ... validation logic ...
}

private void CalculateTotal(Order order)
{
    // ... total calculation logic ...
}
```

## 14. **Extraction de méthode** (Extract Method)
Divise une méthode en plusieurs méthodes plus petites.

***Exemple :***
```CSharp
// Avant
public void ProcessOrder(Order order)
{
    // ... complex logic ...
    // ... many lines of code ...
}

// Après
public void ProcessOrder(Order order)
{
    ValidateOrder(order);
    CalculateTotal(order);
    // ... other steps ...
}

private void ValidateOrder(Order order)
{
    // ... validation logic ...
}

private void CalculateTotal(Order order)
{
    // ... total calculation logic ...
}
```

## 15. **Extraction de superclasse** (Extract Superclass)
Crée une superclasse commune pour plusieurs classes similaires.

***Exemple :***
```CSharp
// Avant
public class Dog
{
    public string Breed { get; set; }
    // ... other dog-specific properties ...
}

public class Cat
{
    public string Breed { get; set; }
    // ... other cat-specific properties ...
}

// Après
public class Animal
{
    public string Breed { get; set; }
    // ... other common properties ...
}

public class Dog : Animal
{
    // ... dog-specific properties ...
}

public class Cat : Animal
{
    // ... cat-specific properties ...
}
```

Bien sûr, continuons avec les autres refactorings :

## 16. **Extraction de variable** (Extract Variable)
Crée une variable pour stocker une expression complexe.

***Exemple :***
```CSharp
// Avant
public double CalculateTotalPrice(double unitPrice, int quantity)
{
    return unitPrice * quantity + CalculateTax(unitPrice * quantity);
}

// Après
public double CalculateTotalPrice(double unitPrice, int quantity)
{
    double basePrice = unitPrice * quantity;
    double tax = CalculateTax(basePrice);
    return basePrice + tax;
}
```

## 17. **Introduction de variable explicative** (Introduce Explaining Variable)
Crée une variable pour clarifier une expression complexe.

***Exemple :***
```CSharp
// Avant
if (order.TotalAmount > 1000 && order.IsUrgent)
{
    // ... complex logic ...
}

// Après
bool isHighValueOrder = order.TotalAmount > 1000;
bool isUrgentOrder = order.IsUrgent;

if (isHighValueOrder && isUrgentOrder)
{
    // ... clearer logic ...
}
```

## 18. **Masquage de délégué** (Hide Delegate)
Cache un objet délégué derrière une méthode.

***Exemple :***
```CSharp
// Avant
class Customer
{
    public Order GetLatestOrder() { ... }
}

// Après
class Customer
{
    private OrderService _orderService;

    public Order GetLatestOrder()
    {
        return _orderService.GetLatestOrder();
    }
}
```

## 19. **Inline de classe** (Inline Class)
Fusionne une classe avec une autre classe.

***Exemple :***
```CSharp
// Avant
class Address { ... }

class Customer
{
    public Address BillingAddress { get; set; }
    // ... other properties ...
}

// Après
class Customer
{
    public string BillingAddress { get; set; }
    // ... other properties ...
}
```

## 20. **Inline de fonction** (Inline Function)
Remplace un appel de fonction par le corps de la fonction.

***Exemple :***
```CSharp
// Avant
public double CalculateTotalPrice(double unitPrice, int quantity)
{
    return GetBasePrice(unitPrice, quantity) + CalculateTax(unitPrice * quantity);
}

private double GetBasePrice(double unitPrice, int quantity)
{
    return unitPrice * quantity;
}

// Après
public double CalculateTotalPrice(double unitPrice, int quantity)
{
    return unitPrice * quantity + CalculateTax(unitPrice * quantity);
}
```

Bien sûr ! Voici une reformulation du contenu avec des exemples de code en C# et un format adapté pour un fichier `readme.md` (Markdown) :

---

## Refactoring de code : Principes et techniques

Le refactoring est une pratique essentielle pour améliorer la qualité du code, réduire la dette technique et maintenir un système logiciel évolutif. Voici quelques techniques courantes de refactoring que vous pouvez appliquer en C# :

### 21. Inline de méthode (Inline Method)
Remplacez un appel de méthode par le corps de la méthode elle-même. Cela peut rendre le code plus lisible et réduire les appels de méthode inutiles.

Exemple en C# :
```csharp
// Avant
public int CalculateTotal(int a, int b)
{
    return Add(a, b);
}

// Après (Inline de méthode)
public int CalculateTotal(int a, int b)
{
    return a + b;
}
```

### 22. Inline de variable (Inline Variable)
Remplacez une variable par sa valeur directement dans le code. Cela peut simplifier le code et éliminer les variables temporaires inutiles.

Exemple en C# :
```csharp
// Avant
int total = CalculateTotal(10, 20);
Console.WriteLine(total);

// Après (Inline de variable)
Console.WriteLine(CalculateTotal(10, 20));
```

### 23. Introduction d’une assertion (Introduce Assertion)
Ajoutez une assertion pour vérifier une condition. Cela renforce la robustesse du code.

Exemple en C# :
```csharp
// Avant
if (quantity > 0)
{
    // Traitement
}

// Après (Introduction d’une assertion)
Debug.Assert(quantity > 0, "La quantité doit être positive.");
// Traitement
```

### 24. Déplacement de méthode (Move Method)
Déplacez une méthode d’une classe à une autre pour améliorer l'organisation du code.

Exemple en C# :
```csharp
// Avant
public class Order
{
    public void CalculateTotal()
    {
        // Calcul du total
    }
}

// Après (Déplacement de méthode)
public class ShoppingCart
{
    public void CalculateTotal(Order order)
    {
        // Calcul du total
    }
}
```

Bien sûr, voici la suite des techniques de refactoring en C# avec des exemples pour chacune d'elles :

### 25. Déplacement d’instructions dans une fonction (Move Statements into Function)
Cette technique consiste à regrouper des instructions dans une nouvelle fonction. Elle permet de rendre le code plus modulaire et de réduire la duplication.

Exemple en C# :
```csharp
// Avant
public void ProcessOrder(Order order)
{
    // Traitement 1
    // Traitement 2
    // ...
}

// Après (Déplacement d’instructions dans une fonction)
public void ProcessOrder(Order order)
{
    ProcessStep1(order);
    ProcessStep2(order);
    // ...
}

private void ProcessStep1(Order order)
{
    // Traitement 1
}

private void ProcessStep2(Order order)
{
    // Traitement 2
}
```

### 26. Déplacement d’instructions vers les appelants (Move Statements to Callers)
Cette technique consiste à déplacer des instructions vers les appelants d’une fonction. Elle permet de réduire la complexité de la fonction et de rendre le code plus lisible.

Exemple en C# :
```csharp
// Avant
public void CalculateTotal(Order order)
{
    // Calcul du total
    // ...
    // Envoi de la facture
}

// Après (Déplacement d’instructions vers les appelants)
public void CalculateTotal(Order order)
{
    // Calcul du total
}

public void SendInvoice(Order order)
{
    // Envoi de la facture
}
```

### 27. Paramétrage de fonction (Parameterize Function)
Cette technique consiste à remplacer des valeurs constantes par des paramètres. Elle permet de rendre le code plus flexible et réutilisable.

Exemple en C# :
```csharp
// Avant
public double CalculateTax(double amount)
{
    const double taxRate = 0.1;
    return amount * taxRate;
}

// Après (Paramétrage de fonction)
public double CalculateTax(double amount, double taxRate)
{
    return amount * taxRate;
}
```

### 28. Paramétrage de méthode (Parameterize Method)
Cette technique consiste à ajouter des paramètres à une méthode. Elle permet de généraliser une méthode pour qu'elle puisse traiter différents cas.

Exemple en C# :
```csharp
// Avant
public void PrintInvoice(Order order)
{
    // Impression de la facture
}

// Après (Paramétrage de méthode)
public void PrintDocument(Document document)
{
    // Impression du document
}
```

### 29. Préservation de l’objet complet (Preserve Whole Object)
Cette technique consiste à passer un objet complet au lieu de ses parties. Elle permet de réduire le nombre de paramètres d'une méthode.

Exemple en C# :
```csharp
// Avant
public void ProcessOrder(int orderId, string customerName, double totalAmount)
{
    // Traitement de la commande
}

// Après (Préservation de l’objet complet)
public void ProcessOrder(Order order)
{
    // Traitement de la commande
}
```

### 30. Remontée du corps du constructeur (Pull Up Constructor Body)
Cette technique consiste à déplacer le corps du constructeur vers une superclasse. Elle permet de réduire la duplication de code dans les sous-classes.

Exemple en C# :
```csharp
// Avant
public class BaseClass
{
    public BaseClass(string name)
    {
        // Initialisation commune
    }
}

public class DerivedClass : BaseClass
{
    public DerivedClass(string name) : base(name)
    {
        // Initialisation spécifique
    }
}

// Après (Remontée du corps du constructeur)
public class BaseClass
{
    public BaseClass(string name)
    {
        // Initialisation commune
    }
}

public class DerivedClass : BaseClass
{
    public DerivedClass(string name) : base(name)
    {
        // Initialisation spécifique
    }
}
```

N'hésitez pas à explorer davantage ces techniques et à les appliquer en fonction de vos besoins spécifiques.
