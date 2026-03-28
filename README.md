<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <title>Sắp xếp theo tổ</title>

  <style>
    body {
      text-align: center;
      font-family: sans-serif;
      background: linear-gradient(135deg, #74ebd5, #ACB6E5);
      min-height: 100vh;
    }

    h2 {
      color: white;
      text-shadow: 1px 1px 3px black;
    }

    .box {
      background: rgba(255,255,255,0.9);
      padding: 15px;
      border-radius: 15px;
      width: 90%;
      margin: auto;
      margin-top: 10px;
    }

    .grid {
      display: grid;
      grid-template-columns: repeat(4, 120px);
      gap: 10px;
      justify-content: center;
      margin-top: 20px;
    }

    .seat {
      border-radius: 10px;
      padding: 10px;
      background: #ffffff;
      box-shadow: 0 3px 6px rgba(0,0,0,0.2);
      font-size: 13px;
      opacity: 0;
      transform: translateY(10px);
      transition: all 0.3s ease;
    }

    .seat.show {
      opacity: 1;
      transform: translateY(0);
    }

    .leader {
      background: gold;
      font-weight: bold;
    }

    input, textarea {
      margin: 5px;
      padding: 6px;
      width: 90%;
      border-radius: 8px;
      border: 1px solid #ccc;
    }

    button {
      margin: 5px;
      padding: 8px 15px;
      border: none;
      border-radius: 10px;
      background: #4CAF50;
      color: white;
      cursor: pointer;
    }

    button:hover {
      background: #45a049;
    }
  </style>
</head>
<body>

<h2>📚 Sắp xếp theo 4 tổ</h2>

<div class="box">
<p>Nhập tên 4 tổ trưởng:</p>
<input id="leader1" placeholder="Tổ trưởng 1"><br>
<input id="leader2" placeholder="Tổ trưởng 2"><br>
<input id="leader3" placeholder="Tổ trưởng 3"><br>
<input id="leader4" placeholder="Tổ trưởng 4"><br>

<p>Nhập danh sách học sinh (mỗi dòng 1 người):</p>
<textarea id="students" rows="10"></textarea>

<br>
<button onclick="start()">Bắt đầu</button>
<button onclick="shuffle()">Xáo lại</button>
</div>

<div class="grid" id="grid"></div>

<script>
let leaders = [];
let students = [];
let groups = [[], [], [], []];

function start() {
  leaders = [
    document.getElementById("leader1").value,
    document.getElementById("leader2").value,
    document.getElementById("leader3").value,
    document.getElementById("leader4").value
  ];

  let input = document.getElementById("students").value;

  students = input
    .split("\n")
    .map(s => s.trim())
    .filter(s => s);

  if (students.length !== 43) {
    alert("⚠️ Cần đúng 43 học sinh!");
    return;
  }

  shuffle();
}

function shuffle() {
  if (students.length === 0) {
    alert("Nhập dữ liệu trước!");
    return;
  }

  groups = [[], [], [], []];

  let shuffled = [...students];
  shuffled.sort(() => Math.random() - 0.5);

  shuffled.forEach((name, index) => {
    groups[index % 4].push(name);
  });

  renderAnimated();
}

function renderAnimated() {
  let grid = document.getElementById("grid");
  grid.innerHTML = "";

  let maxRows = Math.max(...groups.map(g => g.length)) + 1;
  let seats = [];

  for (let row = 0; row < maxRows; row++) {
    for (let col = 0; col < 4; col++) {
      let name = "";

      if (row === 0) {
        name = leaders[col];
      } else {
        name = groups[col][row - 1] || "";
      }

      seats.push({
        name: name,
        leader: row === 0
      });
    }
  }

  // tạo ghế trước
  seats.forEach(s => {
    let div = document.createElement("div");
    div.className = "seat" + (s.leader ? " leader" : "");
    grid.appendChild(div);
  });

  let divs = document.querySelectorAll(".seat");

  // hiện từ từ
  seats.forEach((s, i) => {
    setTimeout(() => {
      divs[i].innerText = s.name;
      divs[i].classList.add("show");
    }, i * 50); // tốc độ hiện
  });
}
</script>

</body>
</html>