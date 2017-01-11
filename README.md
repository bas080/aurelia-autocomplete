# aurelia-autocomplete

[![Build Status](https://travis-ci.org/SpoonX/aurelia-autocomplete.svg)](https://travis-ci.org/SpoonX/aurelia-autocomplete)
[![Gitter](https://img.shields.io/gitter/room/nwjs/nw.js.svg?maxAge=2592000?style=plastic)](https://gitter.im/SpoonX/Dev)

An autocomplete component for the aurelia framework.

## Setup

```bash
jspm install aurelia-autocomplete
```

## Usage

> simple

```html
<auto-complete
  resource='language'>
</auto-complete>
```

> extended

```js
export class MyViewModel {

  results     = [];
  customLabel = product => product.name + product.barcode;
  search      = 'banan';
  productCriteria = {
    stock: {'>': 0}
  };
  defaultProduct = {
    productName: 'No product',
    productCategory   : 'none'
  };

  constructor() {
    product = this.defaultProduct;
  }

  resultsChanged(products) {
    // select the first product that has the name 'banana'. If cannot be found
    // then select the first product in the results.
    this.selected = products.find(product => product.name === 'banana') || products[0];

    // always show the no product option
    this.results = this.results.concat([this.defaultProduct]);
  }

  productSort(products) {
    products.sort((a, b) => a.price > b.price;
  }

}
```

```html

  <h4>${product.productName} ${product.productCategory}</h4>

  <auto-complete
    resource="product"
    selected.bind="selected"
    search.bind="search"
    value.bind="product"
    attribute="productName"
    results.bind="results"
    limit="5"
    endpoint="product-api"
    sort.bind="productSort"
    criteria.bind="{}"
    label.bind="customLabel">
  </auto-complete>

```

## Documentation

### Bindables

#### limit;
the max amount of results to return. (optional)

#### resource;
the string that is appended to the api endpoint. e.g. api.com/language.
language is the resource.

#### search = '';
the string to be used to do a contains search with. By default it will look
if the name contains this value

#### selected;
can be used to select default element visually

#### attribute = 'name';
the property to query on.

#### ({defaultBindingMode: bindingMode.twoWay}) value = null;
used to pass the "selected" value to the user's view model

#### ({defaultBindingMode: bindingMode.twoWay}) results = [];
the results returned from the endpoint. These can be observed and
mutated.

#### label = result => result[this.attribute];
used to determine the string to be shown as option label

#### endpoint;
allow to overwrite the default api endpoint

#### sort = items => items;
sort method that takes a list and returns a sorted list. No sorting by
default.

#### criteria = {};
used to make the criteria more specific


### Methods

Some methods might be handy to overwrite or wrap.

#### shouldPerformRequest()

when it returns true, a request will be performed on a search change.

#### findResults(query)

Overwrite the default fetch. Expects to return a Promise which resolves to
a list of results

#### searchQuery(string)
By default aurelia autocomplete works with waterline queries. If your query
language differs, you can overwrite this method to return a query your endpoint
accepts.
