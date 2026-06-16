---
Author: Sebastian Entleitner
Date: 11.06.20226
---

# RSS Reader - SWE2 Projekt

## Projektbeschriebung

**RSS-Reader**: Der RSS-Reader ermöglicht es, die RSS-Feeds einer beliebigen Zeitung/Informationsquelle einzulesen und in kurzform auszugeben. Somit hat man immer die wichtigsten Nachrichten kurz und knapp im Überblick. 

## XML-Repository

```cs
//RestClient
var client = new RestClient();
var request = new RestRequest(feed);
var response = await client.GetAsync(request);
var contentResponse = response?.Content ?? "";

//Foreach Schleife 
foreach (var be in b)
 {
     XElement node = new XElement("beitrag");
     node.Add(new XAttribute("id", be.Id));
     node.Add(new XAttribute("title", be.Title));
     node.Add(new XAttribute("description", be.Description));
     node.Add(new XAttribute("date", be.DateTime.ToString("yyyy-MM-dd")));
     node.Add(new XAttribute("url", be.Url));
     _rootElement.Add(node);
 }

``` 

## Klassen (Beitrag, Feed)

**Beitrag**

```cs
 public class Beitrag
 {
     //Properties
     public string Id { get; set; } = string.Empty;
     public string Title { get; set; } = string.Empty;
     public string Description { get; set; } = string.Empty;
     public DateTime DateTime { get; set; } = DateTime.Now;
     public string Url { get; set; } = string.Empty;
     public bool IsSaved { get; set; } = false;

     public override string ToString()
     {
         return $"[{DateTime.ToString("dd.MM.yyyy HH:mm")}] {Title}\nLink: {Url}\n";
     }
 }

 ```

 **Feed**

 ```cs
  public class Feed
  {
      
          // Ein lesbarer Name für den Benutzer (z.B. "Tagesschau - Eilmeldungen")
          public string Name { get; set; } = string.Empty;

          // XML-URL des RSS-Feeds
          public string Url { get; set; } = string.Empty;

          //Konstruktor, Feed muss mit name und url erstellt werden
          public Feed(string name, string url)
          {
              Name = name;
              Url = url;
          }

          public override string ToString()
          {
              return $"{Name} ({Url})";
          }
      }

 ```

 ## Singly Linked List

 ```cs

// Hinzufügen

 public void Add(Beitrag beitrag)
 {
     SinglyListNode newNode = new SinglyListNode(beitrag);
     SinglyListNode current = _header;

     while (current.Next != null)
     {
         current = current.Next;
     }
     current.Next = newNode;
 }

//Alle Beiträge ausgeben

  public void PrintAll()
 {
     SinglyListNode current = _header.Next; // Nach dem Header starten
     if (current == null)
     {
         Console.WriteLine("Singly Linked List ist leer.");
         return;
     }
     while (current != null)
     {
         Console.WriteLine($"[SLL - {current.Data.DateTime.ToString("yyyy-MM-dd")}] {current.Data.Title}");
         current = current.Next;
     }
 }


 // SLL Node

 public class SinglyListNode
{
        // Properties
        public Beitrag Data { get; set; }
        public SinglyListNode Next { get; set; }

        // Konstruktor für reguläre Knoten
        public SinglyListNode(Beitrag data)
        {
            Data = data;
            Next = null;
        }

        // Konstruktor für den Header-Knoten
        public SinglyListNode()
        {
            Data = null;
            Next = null;
        }
 }


 ```
 ## Doubly Linked List

 ```cs

 public class DoublyListNode
 {
     public Beitrag Data { get; set; }
     public DoublyListNode Next { get; set; }
     public DoublyListNode Previous { get; set; }

     // Konstruktor für reguläre Knoten
     public DoublyListNode(Beitrag data)
     {
         Data = data;
         Next = null;
         Previous = null;
     }

     // Konstruktor für die Dummy-Knoten
     public DoublyListNode()
     {
         Data = null;
         Next = null;
         Previous = null;
     }
 }


 // Einfügen am Ende (direkt vor dem Tail-Element)
 public void Add(Beitrag beitrag)
 {
     DoublyListNode newNode = new DoublyListNode(beitrag);
     DoublyListNode lastRealNode = _tail.Previous;

     lastRealNode.Next = newNode;
     newNode.Previous = lastRealNode;
     newNode.Next = _tail;
     _tail.Previous = newNode;
 }



  // Rückwärts durchlaufen
 public void PrintBackward()
 {
     DoublyListNode current = _tail.Previous;
     if (current == _header)
     {
         Console.WriteLine("Doubly Linked List ist leer.");
         return;
     }
     while (current != _header)
     {
         Console.WriteLine($"[DLL Backward - {current.Data.DateTime.ToString("yyyy-MM-dd")}] {current.Data.Title}");
         current = current.Previous;
     }
 }

 ```

 ## Binary Search Tree

 ```cs
 private TreeNode InsertRec(TreeNode root, Beitrag beitrag)
 {
     if (root == null)
     {
         return new TreeNode(beitrag);
     }

     // Sicheres Auslesen der Anfangsbuchstaben (In Großbuchstaben umgewandelt für korrekte Sortierung)
     char neuerBuchstabe = string.IsNullOrEmpty(beitrag.Title) ? ' ' : char.ToUpper(beitrag.Title[0]);
     char aktuellerBuchstabe = string.IsNullOrEmpty(root.Data.Title) ? ' ' : char.ToUpper(root.Data.Title[0]);

     // Wenn das Zeichen im Alphabet weiter vorne steht -> nach links gehen
     if (neuerBuchstabe < aktuellerBuchstabe)
     {
         root.Left = InsertRec(root.Left, beitrag);
     }
     // Wenn das Zeichen gleich ist oder weiter hinten steht -> nach rechts gehen
     else
     {
         root.Right = InsertRec(root.Right, beitrag);
     }

     return root;
 }


     // Startet die sortierte Ausgabe
    public void PrintInOrder()
    {
        if (_root == null)
        {
            Console.WriteLine("Der Baum ist leer.");
            return;
        }
        PrintInOrderRec(_root);
    }

    // Besucht erst links (A), dann aktuell, dann rechts (Z)
    private void PrintInOrderRec(TreeNode root)
    {
        if (root != null)
        {
            PrintInOrderRec(root.Left);

            // Holt den Anfangsbuchstaben für das Präfix
            char anfangsBuchstabe = string.IsNullOrEmpty(root.Data.Title) ? '?' : char.ToUpper(root.Data.Title[0]);
            Console.WriteLine($"[BST Alphabetisch - {anfangsBuchstabe}] {root.Data.Title} ({root.Data.DateTime:yyyy-MM-dd})");

            PrintInOrderRec(root.Right);
        }
    }

 ``` 

 **Stream Writer**

 ```cs
 public void WriteToStream(StreamWriter sw, bool asCsv)
 {
     WriteToStreamRec(_root, sw, asCsv);
 }

 // Hilfsmethode für Stream-Writer
 private void WriteToStreamRec(TreeNode root, StreamWriter sw, bool asCsv)
 {
     if (root != null)
     {
         WriteToStreamRec(root.Left, sw, asCsv);
         if (asCsv)
         {
             sw.WriteLine($"{root.Data.DateTime:yyyy-MM-dd};\"{root.Data.Title}\";\"{root.Data.Url}\"");
         }
         else
         {
             char anfangsBuchstabe = string.IsNullOrEmpty(root.Data.Title) ? '?' : char.ToUpper(root.Data.Title[0]);
             sw.WriteLine($"[{anfangsBuchstabe}] {root.Data.Title} ({root.Data.DateTime:yyyy-MM-dd})");
         }
         WriteToStreamRec(root.Right, sw, asCsv);
     }
 }

 ``` 
