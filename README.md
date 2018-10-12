# flutter_course Notes

Notes taken from the course

## Getting Started

Align Vertically -- mainAxisAlignment: MainAxisAlignment.center
Align Horizontally -- crossAxisAlignment: CrossAxisAlignment.center

#### Using Objects (Map)

Map works like Map<KeyType, ValueType>

Map<String, dynamic> for multiple types

#### OTher stuff
ButtonBar -- Add multiple buttons side by side

Container -- Can set height manually

Expanded -- Sort of like container but takes up the rest of the screen height. (Dynamic)

ListView.builder -- Best way to render unknown sized list - Loads when needed, destroys when out of screen.

##### Navigation

- Hamburger menu is called **drawer**. drawer is for left side and drawerend is right side.

class ProductsPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return Scaffold(
      drawer: Drawer(
        child: Column(
          children: <Widget>[
            AppBar(
              automaticallyImplyLeading: false,
              title: Text('Choose'),
            ),
            ListTile(
              title: Text('Manage Products'),
              onTap: () {},
            ),
          ],
        ),
      ),
      appBar: AppBar(
        title: Text('EasyList'),
      ),
      body: ProductManager(),
    );
  }
}

-----

- Navigate to with back navigation -> Navigator.push(context, MaterialPageRoute(builder: (BuildContext context) => ProductPage()))
- Navigate to without possibility to back -> Navigator.pushReplacement(context, MaterialPageRoute(builder: (BuildContext context) => ProductsPage()))
- Navigate back -> Navigator.pop(context)

>**Listen for The Result Of The Navigation**

> return WillPopScope(onWillPop: () {
        Navigator.pop(context, false);
        return Future.value(false); // Allows to leave page.
      },
      child: Scaffold(..)
    }
  )
  
- This will listen for clicks on the back button.

> Navigator.push<returntype>(.....)**.then((Type returnedValue) = {
  .. Execute stuff.
}

# Rendering Lists


- Column tries to squeeze all elements onto one page.

- Use ListView to render a scrollable list.

- ListView must be wrappet into a fixed-height container or Expandable Widget if
it's not the only Widget on the screen!

- Use ListView(children: ...) for short lists where you know the amount of items in advance (example; user input forms)



# Rendering Conditional Content

- Use ternary expressions to render different widgets ( return  x == 0 ? "it was true" : "it was false")

- Do not return null, use empty Container() instead.

- Use if-statements instead of ternary for more complex conditions or widget trees

### Code Below will render a **LARGE** list in the most performant way - Loading when needed and destroying when scrolling out of picture.

``` 
 Products([this.products = const []]) {
    print('[Products Widget] Constructor');
  }

  Widget _buildProductItem(BuildContext context, int index) {
    return Card(
        child: Column(
          children: <Widget>[
            Image.asset('assets/food.jpg'),
            Text(products[index])
          ],
        ),
      );

  }

  @override
  Widget build(BuildContext context) {
    print('[Products Widget] build()');
    return ListView.builder(
      itemBuilder: _buildProductItem,
      itemCount: products.length
    );
  }
  ```


### Conditional Rendering

## To not render nothing, simply return an Empty Container().


#### With turnary for simpler cases
```   
@override
  Widget build(BuildContext context) {
    return products.length > 0 ? ListView.builder(
      itemBuilder: _buildProductItem,
      itemCount: products.length,
    ) : Center(child: Text('No products found, please add some'),);
  }
```
  
#### For more Complex Cases
```
@override
  Widget build(BuildContext context) {
    Widget productCard = Center(child: Text('No products found, please add some'),);
    if (products.length > 0) {
      productCard = ListView.builder(
        itemBuilder: _buildProductItem,
        itemCount: products.length,
      );
    }
    return productCard;
 ```   

 #### Third way would be to move it out to functions.

```
   Widget _buildProductLists() {
    Widget productCard = Center(child: Text('No products found, please add some'),);

    if (products.length > 0) {
      productCard = ListView.builder(
        itemBuilder: _buildProductItem,
        itemCount: products.length,
      );
    }

    return productCard;
  }

  @override
  Widget build(BuildContext context) {
    print('[Products Widget] build()');
    return _buildProductLists();
  }
```