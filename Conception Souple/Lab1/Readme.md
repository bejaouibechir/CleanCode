# Scénario 1 : Système de Gestion des Stocks
**Contexte :** Un système basique de gestion des stocks pour une petite entreprise.

## Avant :
**Classe Produit**
- Propriétés : ***Id***, ***Nom***, ***Quantité***
- Méthodes : AjouterStock(int quantite), RetirerStock(int quantite)

## Implémentation Progressive des Principes :
1. Contour conceptuel : Utilisation du pattern Factory pour la création des produits.
```CSharp
public class ProduitFactory
{
    public Produit CreerProduit(int id, string nom, int quantite)
    {
        return new Produit(id, nom, quantite);
    }
}

```
2. Interfaces révélatrices d'intention : Nommer les interfaces et méthodes de manière descriptive.

```CSharp 
public interface IProduitService
{
    void AjouterStock(int produitId, int quantite);
    void RetirerStock(int produitId, int quantite);
}

public class ProduitService : IProduitService
{
    private readonly IProduitRepository _repository;

    public ProduitService(IProduitRepository repository)
    {
        _repository = repository;
    }

    public void AjouterStock(int produitId, int quantite)
    {
        var produit = _repository.GetProduit(produitId);
        produit.AjouterStock(quantite);
        _repository.UpdateProduit(produit);
    }

    public void RetirerStock(int produitId, int quantite)
    {
        var produit = _repository.GetProduit(produitId);
        produit.RetirerStock(quantite);
        _repository.UpdateProduit(produit);
    }
}

```

3. Fonctions enfermées : Utilisation des délégués pour encapsuler des méthodes de manipulation de stock.

```CSharp 
public delegate void GestionStockDelegate(int quantite);

public class Produit
{
    public int Id { get; }
    public string Nom { get; }
    public int Quantite { get; private set; }

    public GestionStockDelegate AjouterStockDelegate;
    public GestionStockDelegate RetirerStockDelegate;

    public Produit(int id, string nom, int quantite)
    {
        Id = id;
        Nom = nom;
        Quantite = quantite;
        AjouterStockDelegate = AjouterStock;
        RetirerStockDelegate = RetirerStock;
    }

    private void AjouterStock(int quantite)
    {
        Quantite += quantite;
    }

    private void RetirerStock(int quantite)
    {
        Quantite -= quantite;
    }
}

``` 

4. Fonctions à effet secondaire : Manipulation directe des valeurs de l'état du produit en dehors de son environnement local.

```CSharp 
public class Produit
{
    public int Id { get; }
    public string Nom { get; }
    public int Quantite { get; private set; }

    public Produit(int id, string nom, int quantite)
    {
        Id = id;
        Nom = nom;
        Quantite = quantite;
    }

    public void AjouterStock(int quantite)
    {
        Quantite += quantite;
        Logger.Log($"Stock ajouté. Nouvelle quantité : {Quantite}");
    }

    public void RetirerStock(int quantite)
    {
        Quantite -= quantite;
        Logger.Log($"Stock retiré. Nouvelle quantité : {Quantite}");
    }
}

public static class Logger
{
    public static void Log(string message)
    {
        Console.WriteLine(message);
    }
}

```

5. Style déclaratif : Utilisation d'attributs pour valider les propriétés du produit.

```CSharp 
public class Produit
{
    [Required]
    public int Id { get; set; }
    [Required]
    [StringLength(100)]
    public string Nom { get; set; }
    [Range(0, int.MaxValue)]
    public int Quantite { get; set; }
}
```
6. Programmation défensive : Ajout de vérifications pour éviter des entrées invalides.

```CSharp 
public class Produit
{
    public int Id { get; private set; }
    public string Nom { get; private set; }
    private int quantite;

    public int Quantite 
    { 
        get => quantite; 
        private set 
        {
            if (value < 0)
                throw new ArgumentOutOfRangeException(nameof(Quantite), "La quantité ne peut pas être négative");
            quantite = value;
        }
    }

    public Produit(int id, string nom, int quantite)
    {
        if (quantite < 0)
            throw new ArgumentOutOfRangeException(nameof(quantite), "La quantité initiale ne peut pas être négative");

        Id = id;
        Nom = nom;
        Quantite = quantite;
    }
}
```







   
