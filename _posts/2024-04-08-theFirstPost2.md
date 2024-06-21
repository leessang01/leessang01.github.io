---
title: 첫 번째 게시글2
date: 2024-04-08 12:00:00 +0000
categories:
  - 블로그
tags:
  - github
  - jekyll
  - chirpy
---
안녕하세요!
첫 번째 게시글입니다.

---

<a href="https://pokemondb.net/pokedex/landorus"><img src="https://img.pokemondb.net/sprites/scarlet-violet/normal/landorus-incarnate.png" alt="Landorus"></a>


---

<table>
<thead>
  <tr>
    <th>ID</th>
    <th>Item Name</th>
    <th>Item Price</th>
    <th>Item Amount</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td><img src="https://img.pokemondb.net/sprites/scarlet-violet/normal/landorus-incarnate.png" alt="Landorus" style="width: 50px;"></td>
    <td>Item X</td>
    <td>$3</td>
    <td>3</td>
  </tr>
</tbody>
</table>

---

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dynamic Table from JSON with Sorting</title>
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/bootstrap-table/1.18.3/bootstrap-table.min.css">
</head>
<body>

<div class="container">
  <h2 class="mb-4">Dynamic Table from JSON with Sorting</h2>
  <table id="table"
         data-toggle="table" 
         data-url="/assets/data1.json"
         data-height="800" 
         data-pagination="true"
         data-search="true"
         class="table table-bordered table-hover">
    <thead>
      <tr>
        <th data-field="name">이름</th>
        <th data-field="fmove" data-sortable="true">일반공격</th>
        <th data-field="cmove" data-sortable="true">차징공격</th>
        <th data-field="dps" data-sortable="true">DPS</th>
        <th data-field="tdo" data-sortable="true">TDO</th>
      </tr>
    </thead>
  </table>
</div>

<script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-table@1.22.6/dist/bootstrap-table.min.css">
<script src="https://cdn.jsdelivr.net/npm/bootstrap-table@1.22.6/dist/bootstrap-table.min.js"></script>
<script>
  $(function() {
    $("#table").bootstrapTable();
  })
</script>

</body>
</html>



