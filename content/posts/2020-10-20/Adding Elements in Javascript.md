---
weight: 1
title: 'Adding Elements In Javascript'
date: 2020-10-20T15:59:01+13:00
draft: false
author: "David"
authorlink: ""
permalink: /posts/2020/10/20/Adding-Elements-in-Javascript
description: "Adding Elements & Nodes using Standard Javascript"
featuredImage: "/images/js-logo.png"
featuredImagePreview: "/images/js-logo.png"

tags: ["Javascript","Elements","DOM","Nodes"]
categories: ["Software","Development","Javascript","Websites"]
lightgallery: false
toc:
  auto: true
---

An exercise in adding Elements and Nodes using Plain Javascript.
<!--more-->

# Adding Nodes & Elements using Javascript

It is relatively straight forwards to add and remove `Nodes` or `Elements` using standard Javascript.

In this example I am going to be adding **Rows** and `columns` to a `table`.

Setting up the document with a Element or Div to add and remove other Nodes.
We will be using the `tbody` Element in the table declaration:

``` html
<input id="addRow" type="button" value="Add Row" />
<table class="table" >
    <thread>
        <tr>
            <th scope="col">Row #</th>
            <th scope="col">Col 1</th>
            <th scope="col">Col 2</th>
            <th scope="col">Col 3</th>
        </tr>
    </thread>
    <tbody id="tableBody">
        <!--Empty table body Element we are adding to-->
    </tbody>
</table>
```

Setting up the Javascript code to pickup a trigger by the user, in this case a `button` will be enough for this example.

``` javascript
var addBtn = document.getElementById("addRow");
addBtn.addEventListener("click", function() 
{
    /*Logic goes here*/
}
```

The table is setup structured like below:

``` html
<tr> 
    <!--The Row Node-->
    <th>
        <!--Node for the Title of the Row-->
    </th>
    <td>
        <!--Node that hold the cell value/data [ 1 ... n ]-->
    </td>
</tr>
```

## Creating Nodes

In order to create `Nodes`, we can use the `DOM` or *`Document Object Model`*, and call the `createElement` function, specifying the new Node name.

``` javascript
document.createElement("newNode");
```

We can also set `Node attributes`, or *`inner-text`*, using the attribute name, or the `innerText` & `innerHTML` function.

``` javascript
var newNode = document.createElement("newNode");
newNode.innerText = "Hello World!";
```

## Adding Nodes to the DOM

Once created, we need to add the new Node to the existing DOM or document object.

This is done by appending the new Node to the DOM with the `appendChild` function on the `Parent Node`.

``` javascript
var parentNode = document.getElementById("parentNode");
parentNode.appendChild(newNode);
```

### Bringing things together

Going back to our example setup. We want to add **Rows** to the **tbody** *Element*.

As before, create the New Node using `createElement` function.
First create the New Row Node:

``` javascript
//First Create the Table Row Node
var newTableRow = document.createElement("tr");
```

We need to also create a header Node, for the Row Node:

``` javascript
//Next create the Row header Node
var newRowHeader = document.createElement("th");
```

Also we need to specify the scope of the header Node to `Row`, as well as applying the Text to the Label for the Row, then we add this Node to the Row node we created above:

``` javascript
//Setting the scope attribute = row. (Required for header Node)
newRowHeader.scope = "row";
//Naming the Row. (Row Label)
newRowHeader.innerText = "Row 1";
//Now add/append the header Node to the parent table Node
newTableRow.appendChild(newRowHeader);
```

Now we proceed to create the Data Nodes, and give them values:

``` javascript
var newRow1 = document.createElement("td");
newRow1.innerText = "Hello World";
```

Speeding through adding the rest of the Row Values, and appending these to the main Row Node:

``` javascript
var newRow2 = document.createElement("td");
newRow2.innerText = "More Text";
var newRow3 = document.createElement("td");
newRow3.innerText = "Final Word";
newTableRow.appendChild(newRow1);
newTableRow.appendChild(newRow2);
newTableRow.appendChild(newRow3);
```

Now to add it to the DOM, we append it to the **tbody** element we setup at the beginning:

``` javascript
document.getElementById("tbody").appendChild(newTableRow);
```

## Summing up

While adding nodes/elements to the DOM is straight forwards, it does take several steps and also takes a bit of time to implement. Thankfully javascript frameworks have mostly solved this.

Jquery streamlined this, and current modern frameworks like React have made this a breeze. 

So what's the point of this guide. Well it is a good exercise to test these processes, and to better understand what these modern frameworks are doing under the hood.

For small applications with just a few dynamic elements vanilla javascript would be fine, but for more complex pages or web applications, frameworks may be your solution.