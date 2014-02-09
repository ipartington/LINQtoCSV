# Getting Started

Reading from a file

1. In your project, add a reference to the LINQtoCSV.dll you generated during Installation.
2. The file will be read into an IEnumerable<t>, where T is a data class that you define. The data records read from the file will be stored in objects of this data class. You could define a data class along these lines:

```c#
  using LINQtoCSV;
  using System;  
  
  class Product
  {
        [CsvColumn(Name = "ProductName", FieldIndex = 1)]
        public string Name { get; set; }
        
        [CsvColumn(FieldIndex = 2, OutputFormat = "dd MMM HH:mm:ss")]
        public DateTime LaunchDate { get; set; }
        
        [CsvColumn(FieldIndex = 3, CanBeNull = false, OutputFormat = "C")]
        public decimal Price { get; set; }
        
        [CsvColumn(FieldIndex = 4)]
        public string Country { get; set; }
        
        [CsvColumn(FieldIndex = 5)]
        public string Description { get; set; }
  }
  ```  

Although this example only uses properties, the library methods will recognize simple fields as well. Just make sure your fields/properties are public.

The optional CsvColumn attribute allows you to specify whether a field/property is required, how it should be written to an output file, etc. Full details are available here.

3. Import the **LINQtoCSV namespace at the top of the source file where you'll be reading the file:

```c#  
    using LINQtoCSV;
```

4. Create a CsvFileDescription object, and initialize it with details about the file that you're going to read. It will look like this:  
```c#   
    CsvFileDescription inputFileDescription = new CsvFileDescription
    {
        SeparatorChar = ',',
        FirstLineHasColumnNames = true
    };
```

This allows you to specify what character is used to separate data fields (comma, tab, etc.), whether the first record in the file holds column names, and a lot more (full details).

5. Create a CsvContext object:
```c# 
        CsvContext cc = new CsvContext();
```
It is this object that exposes the Read and Write methods you'll use to read and write files.  
Read the file into an IEnumerable<t> using the CsvContext object's Read method, like this:
```c# 
            IEnumerable<product>
                products =
                cc.Read<product>
                    ("products.csv", inputFileDescription);
```
This reads the file products.csv into the variable products, which is of type IEnumerable<product>.

You can now access products via a LINQ query, a foreach loop, etc.:
```c# 
        var productsByName =
        from p in products
        orderby p.Name
        select new { p.Name, p.LaunchDate, p.Price, p.Description };

        // or ...
        foreach (Product item in products) { .... }
```
To make it easier to get an overview, here is the code again that reads from a file, but now in one go:

    CsvFileDescription inputFileDescription = new CsvFileDescription
    {
        SeparatorChar = ',',
        FirstLineHasColumnNames = true
    };

    CsvContext cc = new CsvContext();

    IEnumerable<product> products = 
    cc.Read<product>("products.csv", inputFileDescription);

            // Data is now available via variable products.

            var productsByName =
            from p in products
            orderby p.Name
            select new { p.Name, p.LaunchDate, p.Price, p.Description };

            // or ...
            foreach (Product item in products) { .... }
You'll find this same code in the SampleCode project in the sources.
