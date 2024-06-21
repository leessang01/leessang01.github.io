---
title: 첫 번째 게시글
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
    <th>Img</th>
    <th>Name</th>
    <th>Price</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td><img src="https://chirpy-img.netlify.app/commons/avatar.jpg" alt="Chirpy" style="width: 50px;"/></td>
    <td>Chirpy</td>
    <td>Free</td>
  </tr>
</tbody>
</table>

---
<script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
<link rel="stylesheet" href="https://cdn.datatables.net/2.0.8/css/dataTables.dataTables.css" />
<script src="https://cdn.datatables.net/2.0.8/js/dataTables.js"></script>

<table id="ranking_table" style="width:100%;">
<thead></thead>
<tfoot></tfoot>
<tbody></tbody>
</table>

<script>


var list = [];

fetch();

function fetch() {
	$.ajax({
		url: "/assets/data1.json",
		dataType: 'json',
		async: false,
		success: function (data) {
			$.each(data, function(i){
				var pkmInstance = {};
				pkmInstance.name = data[i].name;
				pkmInstance.fmove = data[i].fmove;
				pkmInstance.cmove = data[i].cmove;
				pkmInstance.dps = data[i].dps;
				pkmInstance.tdo = data[i].tdo;
				pkmInstance.img = data[i].img;
				list.push(pkmInstance);
			});
			

		},
		complete: function () {}
	});
}

function round(value, numDigits) {
	var multiplier = Math.pow(10, parseInt(numDigits) || 0);
	return Math.round(value * multiplier) / multiplier;
}

$(function(){
//	$($("#ranking_table").DataTable().column(5).header()).text('ER');
	var table = $("#ranking_table").DataTable({
		lengthChange: false,
		autoWidth: false,
		deferRender: true,
		columns: [
			{ title: "Image", data: "img", width: "10%" },
			{ title: "Pokemon", data: "name", width: "14%" },
			{ title: "Fast Move", data: "fmove", width: "18%" },
			{ title: "Charged Move", data: "cmove", width: "18%" },
			{ title: "DPS", data: "dps", type: "num", width: "10%", orderSequence: ["desc", "asc"] },
			{ title: "TDO", data: "tdo", type: "num", width: "10%", orderSequence: ["desc", "asc"] },
			{ title: "ER", data: "overall", type: "num", width: "10%", orderSequence: ["desc", "asc"] },
		],
		scrollX: true
	});

	for (let pkm of list) {
//		pkm.img = "x";
		pkm.overall = round((pkm.dps ** 3 * pkm.tdo) ** 0.25, 2);
		table.row.add(pkm);
	}
	table.order([3, 'desc']);
	table.draw();
});
</script>



