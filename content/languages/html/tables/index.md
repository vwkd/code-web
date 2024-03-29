---
title: Tables
author: vwkd
index: 4
tags:
  - languages
  - html
---

- `<table>`: table
- `<tr>`: table row, columns are dynamically created by `<td>`, `<th>`
- `<th>`: table header, by default bold and centered
- `<td>`: table data cell, can contain any type of element
- `<thead>`, `<tbody>`, `<tfoot>`: semantically group content, if omitted `<tbody>` is automatically inserted
- beware: use table for semantics of content, not for presentation of page, e.g. layout ❗️

```html
<table>
  <caption>Monthly costs</caption>
  <thead>
    <tr>
      <th>Item</th>
      <th>Price</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>Banana</td>
      <td>$56.75</td>
    </tr>
    <tr>
      <td>Yogurt</td>
      <td>$12.99</td>
    </tr>
  </tbody>

  <tfoot>
    <tr>
      <td>Total</td>
      <td>$69.74</td>
    </tr>
  </tfoot>
</table>
```

<!-- Demo: HTML/table -->



## Attributes

- `colspan`: cell spanning multiple columns
- `rowspan`: cell spanning multiple rows

```html
<table>
  <caption>Monthly costs</caption>
  <thead>
    <tr>
      <th>Item</th>
      <th>Price</th>
      <th>Price new</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>Banana</td>
      <td>$56.75</td>
      <td>$57.00</td>
    </tr>
    <tr>
      <td>Yogurt</td>
      <td colspan="2">$12.99</td>
    </tr>
  </tbody>

  <tfoot>
    <tr>
      <td rowspan="2">Total</td>
      <td>$69.74</td>
      <td>$69.99</td>
    </tr>
    <tr>
      <td>$0</td>
      <td>$0.25</td>
    </tr>
  </tfoot>
</table>
```